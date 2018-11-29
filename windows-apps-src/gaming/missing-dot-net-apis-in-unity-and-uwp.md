---
title: API .NET manquantes dans Unity et UWP
description: Découvrez les API.NET manquantes lors de la création de jeuxUWP dans Unity, ainsi que des solutions de contournement pour les problèmes courants.
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.date: 2/21/2018
ms.topic: article
keywords: windows10, uwp, jeux, .net, unity
ms.localizationpriority: medium
ms.openlocfilehash: 323d710a18ab738f89ed691cd56309ae827a7a14
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7981384"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>API .NET manquantes dans Unity et UWP

Lorsque vous créez un jeu UWP à l’aide de .NET, vous pouvez constater que certaines API susceptibles d’être utilisées dans l’éditeur Unity ou pour un jeu autonome pour PC ne sont pas disponibles pour UWP. Cela s’explique par le fait que .NET pour les applications UWP inclut un sous-ensemble des types fournis dans .NETFramework pour chaque espace de noms.

En outre, certains moteurs de jeu utilisent d’autres versions de .NET qui ne sont pas entièrement compatibles avec .NET pour UWP, telles que Mono de Unity. Par conséquent, lorsque vous écrivez votre jeu, tout peut fonctionner correctement dans l’éditeur, mais lorsque vous créez le jeu pour UWP, vous pouvez obtenir des erreurs telles que: **Le type ou l’espace de noms «Formatters» n’existe pas dans l’espace de noms «System.Runtime.Serialization» (une référence d’assembly est-elle manquante?)**

Heureusement, Unity fournit certaines des API manquantes en tant que méthodes d’extension et types de remplacement, comme décrit dans [Plateforme Windows universelle: types .NET manquants sur le serveur principal de script.NET](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html). Toutefois, si la fonctionnalité dont vous avez besoin n’est pas disponible ici, la rubrique [Vue d’ensemble de .NET pour les applications Windows8.x](https://msdn.microsoft.com/library/windows/apps/br230302) explique comment vous pouvez convertir votre code pour utiliser WinRT ou .NET pour les API UWP. (Elle décrit Windows8, mais s’applique également aux applications UWP Windows10.)

## <a name="net-standard"></a>.NET Standard

Pour comprendre pourquoi certaines API ne fonctionnent pas, il est important d’assimiler les différentes versions de .NET et la façon dont UWP implémente .NET. [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) est une spécification formelle d’API .NET qui est conçue pour être interplateforme et unifier les différentes versions de .NET. Chaque implémentation de .NET prend en charge une version particulière de .NET Standard. Un tableau des normes et des implémentations est disponible dans [Prise en charge des implémentations de.NET](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support).

Chaque version du kit de développement logiciel (SDK) UWP est conforme à un niveau différent de .NET Standard. Par exemple, le kit de développement logiciel (SDK)16299 (FallCreatorsUpdate) prend en charge .NETStandard2.0.

Si vous souhaitez vérifier si une API .NET spécifique est prise en charge dans la version UWP que vous ciblez, vous pouvez vous reporter à [Référence d’API .NET Standard](https://docs.microsoft.com/dotnet/api/index?view=netstandard-2.0) et sélectionner la version de .NET Standard qui est prise en charge par cette version d’UWP.

## <a name="scripting-backend-configuration"></a>Configuration du serveur principal de script

La première chose que vous devez faire lorsque vous rencontrez des problèmes lors de la création d’une application pour UWP est de vérifier les **Paramètres du lecteur** (**Fichier > Paramètres de Build**, sélectionnez **Plateforme Windows universelle**, puis **Paramètres du lecteur**). Sous **Autres paramètres > Configuration**, les trois premières listes déroulantes (**Scripting Runtime Version**, **Scripting Backend** et **Api Compatibility Level**) sont des paramètres qu’il est important de prendre en compte.

La **version d’exécution de script** (Scripting Runtime Version) est utilisée par le serveur de script principal Unity pour vous permettre d’obtenir (à peu près) la version de .NETFramework équivalente à celle que vous choisissez. Toutefois, n’oubliez pas que toutes les API de cette version de .NETFramework ne seront pas prises en charge, mais uniquement celles de la version de .NETStandard ciblée par votre UWP.

Lors de la publication de nouvelles versions de .NET, de nouvelles API sont souvent ajoutées à .NETStandard et celles-ci peuvent vous permettre d’utiliser le même code dans une application autonome et UWP. Par exemple, l’espace de noms [System.Runtime.Serialization.Json](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json) a été introduit dans .NETStandard2.0. Si vous définissez la valeur de **Scripting Runtime Version** sur **.NET 3.5 Equivalent** (qui cible une version antérieure de .NETStandard), vous obtiendrez une erreur lorsque vous essayez d’utiliser l’API; pour que l’API fonctionne, basculez la version sur **.NET 4.6 Equivalent** (qui prend en charge .NETStandard2.0).

La valeur de **Scripting Backend** peut être **.NET** ou **IL2CPP**. Dans cette rubrique, nous partons du principe que vous avez choisi **.NET**, puisqu’il s’agit de la version où apparaissent les problèmes décrits ici. Voir [Scripting Backends](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html) pour en savoir plus.

Enfin, vous devez définir la valeur de **Api Compatibility Level** sur la version de .NET sur laquelle votre jeu doit s’exécuter. Elle doit correspondre à la valeur de **Scripting Runtime Version**.

En règle générale, pour **Scripting Runtime Version** et **Api Compatibility Level**, vous devez sélectionner la dernière version disponible, pour obtenir une meilleure compatibilité avec .NETFramework et ainsi, pouvoir utiliser un plus grand nombre d’API .NET.

![Configuration: Scripting Runtime Version; Scripting Backend; Api Compatibility Level](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>Compilation dépendante de la plateforme

Si vous créez un jeu Unity destiné à plusieurs plateformes, y compris UWP, vous utiliserez une compilation dépendante de la plateforme pour vous assurer que le code destiné à UWP est exécuté uniquement lorsque le jeu est conçu en tant que jeu UWP. De cette façon, vous pouvez utiliser .NETFramework dans son ensemble pour les jeux de bureau autonomes et pour d’autres plateformes et des API WinRT pour UWP, sans erreur de génération.

Utilisez les directives suivantes pour compiler le code uniquement lors de l’exécution en tant qu’application UWP:

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE` sert uniquement à vérifier si vous compilez du code c# sur le serveur principal de script .NET. Si vous utilisez un autre serveur principal de script tel que IL2CPP, utilisez `UNITY_WSA_10_0` à la place.

Pour obtenir la liste complète des directives de compilation dépendante de la plateforme, voir [Platform dependent compilation](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html).

## <a name="common-issues-and-workarounds"></a>Problèmes courants et solutions de contournement

Les scénarios suivants décrivent les problèmes courants pouvant survenir lorsque les API .NET sont manquantes dans le sous-ensemble UWP et les solutions de contournement.

### <a name="data-serialization-using-binaryformatter"></a>Sérialisation des données à l’aide de BinaryFormatter

Les jeux sérialisent souvent les données de sauvegarde afin que les utilisateurs ne soient pas en mesure de les manipuler aisément. Toutefois, [BinaryFormatter](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter), qui sérialise un objet en binaire, n’est pas disponible dans les versions antérieures de .NETStandard (avant la version2.0). Envisagez d’utiliser [XmlSerializer](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer) ou [DataContractJsonSerializer](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer) à la place.

```csharp
private void Save()
{
    SaveData data = new SaveData(); // User-defined object to serialize

    DataContractJsonSerializer serializer = 
      new DataContractJsonSerializer(typeof(SaveData));

    FileStream stream = 
      new FileStream(Application.persistentDataPath, FileMode.CreateNew);

    serializer.WriteObject(stream, data);
    stream.Dispose();
}
```

### <a name="io-operations"></a>Opérations d’E/S

Quelques types de l’espace de noms [System.IO](https://docs.microsoft.com/dotnet/api/system.io), par exemple [FileStream](https://docs.microsoft.com/dotnet/api/system.io.filestream), ne sont pas disponibles dans les versions précédentes de .NET Standard. Toutefois, Unity fournit les types [Directory](https://docs.microsoft.com/dotnet/api/system.io.directory), [File](https://docs.microsoft.com/dotnet/api/system.io.file) et **FileStream** afin que vous les utilisiez dans votre jeu.

Vous pouvez également utiliser les API [Windows.Storage](https://docs.microsoft.com/uwp/api/Windows.Storage), uniquement disponibles pour les applications UWP. Toutefois, ces API contraignent l’application à écrire dans leur stockage spécifique et ne permettent pas d’accéder librement à l’ensemble de leur système de fichiers. Voir [Fichiers, dossiers et bibliothèques](https://docs.microsoft.com/windows/uwp/files/) pour plus d’informations.

Il est important de noter que la méthode [Close](https://docs.microsoft.com/dotnet/api/system.io.stream.close) est uniquement disponible dans .NETStandard2.0 et versions ultérieures (même si Unity fournit une méthode d’extension). Utilisez [Dispose](https://docs.microsoft.com/dotnet/api/system.io.stream.dispose) à la place.

### <a name="threading"></a>Threads

Quelques types de l’espace de noms [System.Threading](https://docs.microsoft.com/dotnet/api/system.threading), par exemple [ThreadPool](https://docs.microsoft.com/dotnet/api/system.threading.threadpool), ne sont pas disponibles dans les versions précédentes de .NETStandard. Dans ces cas, vous pouvez utiliser l’espace de noms [Windows.System.Threading](https://docs.microsoft.com/uwp/api/windows.system.threading) à la place.

Voici comment vous pouvez gérer les threads dans un jeu Unity, en utilisant la compilation dépendante de la plateforme pour préparer le jeu pour les plateformes UWP et non UWP:

```csharp
private void UsingThreads()
{
#if NETFX_CORE
    Windows.System.Threading.ThreadPool.RunAsync(workItem => SomeMethod());
#else
    System.Threading.ThreadPool.QueueUserWorkItem(workItem => SomeMethod());
#endif
}
```

### <a name="security"></a>Sécurité

Parmi les espaces de noms **System.Security. ***, tels que [System.Security.Cryptography.X509Certificates](https://docs.microsoft.com/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0), certains ne sont pas disponibles lorsque vous créez un jeu Unity pour UWP. Dans ce cas, utilisez les API **Windows.Security.*** qui couvrent la majeure partie de la même fonctionnalité.

L’exemple suivant récupère simplement les certificats à partir d’un magasin de certificats avec le nom donné:

```cs
private async void GetCertificatesAsync(string certStoreName)
    {
#if NETFX_CORE
        IReadOnlyList<Certificate> certs = await CertificateStores.FindAllAsync();
        IEnumerable<Certificate> myCerts = 
            certs.Where((certificate) => certificate.StoreName == certStoreName);
#else
        X509Store store = new X509Store(certStoreName, StoreLocation.CurrentUser);
        store.Open(OpenFlags.OpenExistingOnly);
        X509Certificate2Collection certs = store.Certificates;
#endif
    }
```

Consultez [Sécurité](https://docs.microsoft.com/windows/uwp/security/) pour plus d'informations sur les API de sécurité WinRT.

### <a name="networking"></a>Réseaux

Parmi les espaces de noms **System&period;Net.***, tels que [System.Net.Mail](https://docs.microsoft.com/dotnet/api/system.net.mail?view=netstandard-2.0), certains ne sont pas disponibles lorsque vous créez un jeu Unity pour UWP. Pour la plupart de ces API, utilisez les API WinRT correspondantes **Windows.Networking. *** et **Windows.Web. *** pour obtenir une fonctionnalité similaire. Consultez [Réseaux et services web](https://docs.microsoft.com/windows/uwp/networking/) pour plus d’informations.

Dans le cas de **System.Net. Messagerie**, utilisez l'espace de noms [Windows.ApplicationModel.Email](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email). Consultez [Envoyer un e-mail](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email) pour plus d'informations.

## <a name="see-also"></a>Articles associés

* [Universal Windows Platform: Missing .NET Types on .NET Scripting Backend](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [Présentation de .NET pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/br230302)
* [Guides de portage UWP Unity](https://unity3d.com/partners/microsoft/porting-guides)