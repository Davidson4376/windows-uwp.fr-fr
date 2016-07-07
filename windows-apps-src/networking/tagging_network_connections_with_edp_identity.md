---
author: DelfCo
Description: "Cette rubrique vous explique comment générer un contexte de thread protégé avant de créer les connexions réseau dans un scénario EDP."
MS-HAID: dev\_networking.tagging\_network\_connections\_with\_edp\_identity
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Marquage de connexions réseau avec l’identitéEDP"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 2b960bbb5cf58991778e5c20bb915a202ecf6e04

---

# Marquage de connexions réseau avec l’identitéEDP

__Remarque__ La stratégie de protection des données d’entreprise (EDP) ne peut pas être appliquée sur Windows 10, version 1511 (build 10586) ou antérieure.

Cette rubrique vous explique comment générer un contexte de thread protégé avant de créer les connexions réseau dans un scénario EDP. Pour un aperçu complet du point de vue des développeurs de la manière dont la fonctionnalité EDP est liée aux fichiers, aux flux, au Presse-papiers, à la mise en réseau, aux tâches en arrière-plan et à la protection des données verrouillées, voir [protection des données d’entreprise (EDP)](../enterprise/edp-hub.md).

**Remarque** L’[exemple de protection des données d’entreprise (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) aborde plusieurs scénarios EDP.



## Prérequis


-   **Préparation pour la configuration de la fonctionnalité EDP**

    Voir [Configurer votre ordinateur pour EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Impliquez-vous dans la création d’une application spécifique aux entreprises**

    Une application qui assure de manière autonome que les données d’entreprise restent sous le contrôle des entreprises est véritablement spécifique aux entreprises. Une application spécifique aux entreprises est plus puissante, plus intelligente, plus flexible et plus fiable qu’une application ordinaire. Votre application indique au système qu’elle est spécifique en déclarant la fonctionnalité **enterpriseDataPolicy** restreinte. Mais configurer une fonctionnalité ne suffit pas à établir la spécificité. Pour en savoir plus, voir [Applications spécifiques aux entreprises](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour savoir comment écrire des applications asynchrones en C\# ou Visual Basic, voir [Appeler des API asynchrones en C\# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour savoir comment écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Accès aux ressources d’entreprise sur le réseau


Dans ce scénario, une application de messagerie compatible synchronise un ensemble de boîtes aux lettres d’entreprise et personnelles. L’application transmet l’identité de l’utilisateur à un appel [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) afin de créer un contexte de thread protégé. Cette opération marque l’ensemble des connexions réseau effectuées par la suite sur le même thread avec cette identité, et autorise l’accès aux ressources de réseau d’entreprise dont l’accès est contrôlé par la stratégie d’entreprise.

Ici, « enterprise » fait référence à l’entreprise d’appartenance de l’identité de l’utilisateur. [**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) renvoie un objet [**ThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706029), indépendamment de l’application de stratégie. Généralement, si l’application doit gérer des ressources mixtes, elle peut appeler **CreateCurrentThreadNetworkContext** pour l’ensemble des identités. Après avoir récupéré les ressources réseau, l’application appelle **Dispose** sur l’élément **ThreadNetworkContext** afin d’effacer tout marquage d’identité sur le thread actuel. Le modèle utilisé pour disposer l’objet contextuel dépend de votre langage de programmation.

Si l’identité est inconnue, l’application peut interroger l’identité gérée par la stratégie d’entreprise à partir de l’adresse réseau de la ressource, à l’aide de l’API [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027).

**Remarque** Comme vous le remarquez dans l’exemple de code, le modèle approprié d’utilisation de [**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) consiste à limiter au maximum son périmètre d’action. Vous devez définir le contexte de réseau d’entreprise, créer des connexions réseau, puis rétablir le contexte, utiliser les connexions et les fermer. L’exemple de code suivant illustre les détails. Il est important de ne pas créer des fichiers sur un thread pendant que le contexte de réseau d’entreprise est défini sur ce thread. Cela entraînera le chiffrement automatique du fichier, que vous souhaitiez garder ce fichier personnel ou non. C’est l’une des raisons pour lesquelles nous vous recommandons de rétablir le contexte le plus tôt possible.



```CSharp
using Windows.Data.Xml.Dom;
using Windows.Networking;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;
using Windows.Web.Http;

...

public static async void SyncMailbox(string identity)
{
    HttpClient client = null;
    // Create a protected network context for "identity" on the current
    // thread. Network connections made after this call will be tagged
    // to "identity", and will have policy enforced. This is required
    // to access enterprise network resources for "identity".
    using (ThreadNetworkContext threadNetworkContext = 
        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        // Retrieve new mail.
        client = new HttpClient();
    }

    string xmlResponse = await client.GetStringAsync
        (new Uri("https://contosomail/getnewmail?user=" + identity));

    // Now, process mail data received.
    var xmlDocument = new XmlDocument();
    xmlDocument.LoadXml(xmlResponse);
    foreach (IXmlNode mailNode in xmlDocument.GetElementsByTagName("mail"))
    {
        // Code to process text data in mail (body, recipients etc.)
        // would go here ...

        // Processes resource links in mail body.
        foreach (IXmlNode childNode in mailNode.ChildNodes)
        {
            if (childNode.NodeName == "resource")
            {
                var resourceUri = new Uri(childNode.InnerText);

                // Check if the resource is on an enterprise-managed endpoint.
                string resourceIdentity =
                    await ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync
                        (new HostName(resourceUri.Host));
                if (!string.IsNullOrEmpty(resourceIdentity))
                {
                    using (ThreadNetworkContext threadNetworkContext =
                        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(resourceIdentity))
                    {
                        client = new HttpClient();
                    }
                    IBuffer data = await client.GetBufferAsync(resourceUri);
                    // Code to process retrieved resource data would go here...
                }
            }
        }
    }
}
```

**Remarque** Cet article s’adresse aux développeurs de Windows 10 qui créent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).



## Rubriques connexes


[exemple de protection des données d’entreprise (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Espace de noms Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 






<!--HONumber=Jun16_HO4-->


