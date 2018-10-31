---
title: Partager des certificats entre applications
description: Les applications de plateforme Windows universelle (UWP) qui nécessitent une authentification sécurisée au-delà d’une combinaison identifiant utilisateur et mot de passe peuvent utiliser des certificats à des fins d’authentification.
ms.assetid: 159BA284-9FD4-441A-BB45-A00E36A386F9
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: 044b1b60b80cec1fc40adda6b9b6d44bee34ce7c
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5860723"
---
# <a name="share-certificates-between-apps"></a>Partager des certificats entre applications




Les applications de plateforme Windows universelle (UWP) qui nécessitent une authentification sécurisée au-delà d’une combinaison identifiant utilisateur et mot de passe peuvent utiliser des certificats à des fins d’authentification. L’authentification par certificat permet d’authentifier un utilisateur avec un niveau de confiance élevé. Dans certains cas, un groupe de services peut authentifier un utilisateur pour plusieurs applications. Cet article montre comment authentifier plusieurs applications à l’aide du même certificat. Vous apprendrez également à écrire du code pour permettre à un utilisateur d’importer un certificat fourni pour accéder à des services web sécurisés.

Les applications peuvent s’authentifier auprès d’un service web à l’aide d’un certificat, et plusieurs applications peuvent utiliser un même certificat du magasin de certificats pour authentifier le même utilisateur. Si un certificat n’existe pas dans le magasin, vous pouvez ajouter du code à votre application pour importer un certificat à partir d’un fichier PFX.

## <a name="enable-microsoft-internet-information-services-iis-and-client-certificate-mapping"></a>Activer Microsoft Internet Information Services (IIS) et le mappage des certificats clients


Cet article utilise Microsoft Internet Information Services (IIS) à titre d’exemple. IIS n’est pas activé par défaut. Vous pouvez activer IIS via le Panneau de configuration.

1.  Ouvrez le Panneau de configuration, puis sélectionnez **Programmes**.
2.  Sélectionnez **Activer ou désactiver des fonctionnalités Windows**.
3.  Développez **Internet Information Services**, puis **Services World Wide Web**. Développez **Fonctionnalités de développement d’applications**, puis sélectionnez **ASP.NET 3.5** et **ASP.NET 4.5**. Ces sélections permettent d’activer automatiquement **Internet Information Services**.
4.  Cliquez sur **OK** pour appliquer les modifications.

## <a name="create-and-publish-a-secured-web-service"></a>Créer et publier un service web sécurisé


1.  Exécutez Microsoft Visual Studio en tant qu’administrateur et sélectionnez **Nouveau projet** dans la page de démarrage. Un accès administrateur est requis pour publier un service web sur un serveur IIS. Dans la boîte de dialogue Nouveau projet, sélectionnez **.NET Framework3.5**. Sélectionnez **VisualC#** -&gt; **Web** -&gt; **Visual Studio** -&gt; **Application de service Web ASP.NET**. Nommez l’application «FirstContosoBank». Cliquez sur **OK** pour créer le projet.
2.  Dans le fichier **Service1.asmx.cs**, remplacez la méthode Web **HelloWorld** par défaut par la méthode « Login » suivante.
    ```cs
            [WebMethod]
            public string Login()
            {
                // Verify certificate with CA
                var cert = new System.Security.Cryptography.X509Certificates.X509Certificate2(
                    this.Context.Request.ClientCertificate.Certificate);
                bool test = cert.Verify();
                return test.ToString();
            }
    ```

3.  Enregistrez le fichier **Service1.asmx.cs**.
4.  Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur l’application « FirstContosoBank », puis sélectionnez **Publier**.
5.  Dans la boîte de dialogue **Publier le site Web**, créez un profil et nommez-le « ContosoProfile ». Cliquez sur **Suivant**.
6.  Dans la page suivante, entrez le nom de votre serveur IIS, puis spécifiez le nom de site «Default Web Site/FirstContosoBank». Cliquez sur **Publier** pour publier votre service web.

## <a name="configure-your-web-service-to-use-client-certificate-authentication"></a>Configurer votre service web de manière à utiliser l’authentification par certificat client


1.  Exécutez le **Gestionnaire des services Internet (IIS)**.
2.  Développez les sites pour votre serveur IIS. Sous **Site Web par défaut**, sélectionnez le nouveau service web « FirstContosoBank ». Dans la section **Actions**, sélectionnez **Paramètres avancés**.
3.  Choisissez **.NET v2.0** comme **Pool d’applications**, puis cliquez sur **OK**.
4.  Dans le **Gestionnaire des services Internet (IIS)**, sélectionnez votre serveur IIS, puis double-cliquez sur **Certificats de serveur**. Dans la section **Actions**, sélectionnez **Créer un certificat auto-signé**. Saisissez «ContosoBank» comme nom convivial pour le certificat, puis cliquez sur **OK**. Un certificat est alors créé au format « &lt;nom-serveur&gt;.&lt;nom-domaine&gt; » pour le serveur IIS.
5.  Dans le **Gestionnaire des services Internet (IIS)**, sélectionnez le site web par défaut. Dans la section **Actions**, sélectionnez **Liaison**, puis cliquez sur **Ajouter**. Sélectionnez « https » comme type, affectez au port la valeur « 443 », puis entrez le nom d’hôte complet de votre serveur IIS (« &lt;nom-serveur&gt;.&lt;nom-domaine&gt; »). Définissez «ContosoBank» comme certificat SSL. Cliquez sur **OK**. Cliquez sur **Fermer** dans la fenêtre **Liaisons de sites**.
6.  Dans le **Gestionnaire des services Internet (IIS)**, sélectionnez le service web « FirstContosoBank ». Double-cliquez sur **Paramètres SSL**. Cochez **Exiger SSL**. Sous **Certificats clients**, sélectionnez **Demander**. Dans la section **Actions**, sélectionnez **Appliquer**.
7.  Pour vérifier que le service Web est configuré correctement, ouvrez votre navigateur et entrez l’adresse Web suivante : « https://&lt;nom-serveur&gt;.&lt;nom-domaine&gt;/FirstContosoBank/Service1.asmx ». Exemple: «https://myserver.example.com/FirstContosoBank/Service1.asmx». Si votre service web est correctement configuré, vous êtes invité à sélectionner un certificat client pour accéder au service web.

Vous pouvez répéter les étapes précédentes pour créer plusieurs services Web accessibles à l’aide du même certificat client.

## <a name="create-a-uwp-app-that-uses-certificate-authentication"></a>Créer une application UWP qui utilise l’authentification par certificat


Maintenant que vous avez un ou plusieurs services web sécurisés, vos applications peuvent utiliser des certificats pour s’authentifier auprès de ces services web. Lorsque vous faites une demande à un service web authentifié à l’aide de l’objet [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639), la demande initiale ne contient pas de certificat client. Le service Web authentifié répond par une demande d’authentification du client. Lorsque cela se produit, le client Windows interroge automatiquement le magasin de certificats au sujet des certificats clients disponibles. Votre utilisateur peut choisir l’un de ces certificats pour s’authentifier auprès du service Web. Certains certificats étant protégés par mot de passe, vous devez fournir à l’utilisateur un moyen d’entrer le mot de passe associé à un certificat.

Si aucun certificat client n’est disponible, l’utilisateur doit ajouter un certificat au magasin de certificats. Vous pouvez inclure dans votre application du code permettant à un utilisateur de sélectionner un fichier PFX contenant un certificat client, puis importer ce certificat dans le magasin de certificats clients.

**Conseil**vous pouvez utiliser makecert.exe pour créer un fichier PFX à utiliser avec ce démarrage rapide. Pour plus d’informations sur l’utilisation de makecert.exe, voir [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968).

 

1.  Ouvrez Visual Studio et créez un projet à partir de la page de démarrage. Nommez le nouveau projet «FirstContosoBankApp». Cliquez sur **OK** pour créer le projet.
2.  Dans le fichier MainPage.xaml, ajoutez le code XAML suivant à l’élément **Grid** par défaut. Ce code XAML comprend un bouton pour rechercher un fichier PFX à importer, une zone de texte pour entrer un mot de passe pour un fichier PFX protégé par mot de passe, un bouton pour importer un fichier PFX sélectionné, un bouton pour se connecter au service web sécurisé, ainsi qu’un bloc de texte pour afficher l’état de l’action actuelle.
    ```xml
    <Button x:Name="Import" Content="Import Certificate (PFX file)" HorizontalAlignment="Left" Margin="352,305,0,0" VerticalAlignment="Top" Height="77" Width="260" Click="Import_Click" FontSize="16"/>
    <Button x:Name="Login" Content="Login" HorizontalAlignment="Left" Margin="611,305,0,0" VerticalAlignment="Top" Height="75" Width="240" Click="Login_Click" FontSize="16"/>
    <TextBlock x:Name="Result" HorizontalAlignment="Left" Margin="355,398,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Height="153" Width="560"/>
    <PasswordBox x:Name="PfxPassword" HorizontalAlignment="Left" Margin="483,271,0,0" VerticalAlignment="Top" Width="229"/>
    <TextBlock HorizontalAlignment="Left" Margin="355,271,0,0" TextWrapping="Wrap" Text="PFX password" VerticalAlignment="Top" FontSize="18" Height="32" Width="123"/>
    <Button x:Name="Browse" Content="Browse for PFX file" HorizontalAlignment="Left" Margin="352,189,0,0" VerticalAlignment="Top" Click="Browse_Click" Width="499" Height="68" FontSize="16"/>
    <TextBlock HorizontalAlignment="Left" Margin="717,271,0,0" TextWrapping="Wrap" Text="(Optional)" VerticalAlignment="Top" Height="32" Width="83" FontSize="16"/>
    ```
    
3.  Enregistrez le fichier MainPage.xaml.
4.  Dans le fichier MainPage.xaml.cs, ajoutez les instructions using suivantes:
    ```cs
    using Windows.Web.Http;
    using System.Text;
    using Windows.Security.Cryptography.Certificates;
    using Windows.Storage.Pickers;
    using Windows.Storage;
    using Windows.Storage.Streams;
    ```

5.  Dans le fichier MainPage.xaml.cs, ajoutez les variables suivantes à la classe **MainPage**. Celles-ci spécifient l’adresse de la méthode «Login» sécurisée de votre service web «FirstContosoBank». Une variable globale détient aussi un certificat PFX à importer dans le magasin de certificats. Mettez à jour le &lt;nom-serveur&gt; à l’aide du nom complet de votre serveur Microsoft Internet Information Server (IIS).
    ```cs
    private Uri requestUri = new Uri("https://<server-name>/FirstContosoBank/Service1.asmx?op=Login");
    private string pfxCert = null;
    ```

6.  Dans le fichier MainPage.xaml.cs, ajoutez le gestionnaire de clic suivant pour le bouton et la méthode de connexion afin d’accéder au service web sécurisé.
    ```cs
    private void Login_Click(object sender, RoutedEventArgs e)
    {
        MakeHttpsCall();
    }

    private async void MakeHttpsCall()
    {

        StringBuilder result = new StringBuilder("Login ");
        HttpResponseMessage response;
        try
        {
            Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();
            response = await httpClient.GetAsync(requestUri);
            if (response.StatusCode == HttpStatusCode.Ok)
            {
                result.Append("successful");
            }
            else
            {
                result = result.Append("failed with ");
                result = result.Append(response.StatusCode);
            }
        }
        catch (Exception ex)
        {
            result = result.Append("failed with ");
            result = result.Append(ex.Message);
        }

        Result.Text = result.ToString();
    }
    ```

7.  Dans le fichier MainPage.xaml.cs, ajoutez les gestionnaires de clic suivants pour le bouton pour rechercher un fichier PFX et le bouton afin d’importer un fichier PFX sélectionné dans le magasin de certificats.
    ```cs
    private async void Import_Click(object sender, RoutedEventArgs e)
    {
        try
        {
            Result.Text = "Importing selected certificate into user certificate store....";
            await CertificateEnrollmentManager.UserCertificateEnrollmentManager.ImportPfxDataAsync(
                pfxCert,
                PfxPassword.Password,
                ExportOption.Exportable,
                KeyProtectionLevel.NoConsent,
                InstallOptions.DeleteExpired,
                "Import Pfx");

            Result.Text = "Certificate import succeded";
        }
        catch (Exception ex)
        {
            Result.Text = "Certificate import failed with " + ex.Message;
        }
    }

    private async void Browse_Click(object sender, RoutedEventArgs e)
    {

        StringBuilder result = new StringBuilder("Pfx file selection ");
        FileOpenPicker pfxFilePicker = new FileOpenPicker();
        pfxFilePicker.FileTypeFilter.Add(".pfx");
        pfxFilePicker.CommitButtonText = "Open";
        try
        {
            StorageFile pfxFile = await pfxFilePicker.PickSingleFileAsync();
            if (pfxFile != null)
            {
                IBuffer buffer = await FileIO.ReadBufferAsync(pfxFile);
                using (DataReader dataReader = DataReader.FromBuffer(buffer))
                {
                    byte[] bytes = new byte[buffer.Length];
                    dataReader.ReadBytes(bytes);
                    pfxCert = System.Convert.ToBase64String(bytes);
                    PfxPassword.Password = string.Empty;
                    result.Append("succeeded");
                }
            }
            else
            {
                result.Append("failed");
            }
        }
        catch (Exception ex)
        {
            result.Append("failed with ");
            result.Append(ex.Message); ;
        }

        Result.Text = result.ToString();
    }
    ```

8.  Exécutez votre application et établissez une connexion à votre service web sécurisé, et importez un fichier PFX dans le magasin de certificats local.

Vous pouvez utiliser ces étapes pour créer plusieurs applications utilisant le même certificat utilisateur pour accéder à des services web sécurisés identiques ou différents.