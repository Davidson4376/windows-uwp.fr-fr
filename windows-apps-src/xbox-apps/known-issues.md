---
title: Problèmes connus avec UWP dans le programme pour les développeurs Xbox
description: Répertorie les problèmes connus avec UWP dans le programme pour les développeurs Xbox.
ms.date: 03/29/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
ms.localizationpriority: medium
ms.openlocfilehash: 55068ef3f0a0a0d01c61746bde02ddb7aa4ef885
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8923565"
---
# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Problèmes connus avec UWP dans le programme pour les développeurs Xbox

Cette rubrique décrit les problèmes connus liés à la plateforme UWP dans le programme pour les développeurs Xbox. Pour plus d’informations sur ce programme, voir [UWP sur Xbox](index.md). 

\[Si vous avez accédé à cette page à partir d’un lien dans une rubrique de référence d’une API et que vous recherchez des informations sur les API pour les appareils universels, consultez [Fonctionnalités UWP non encore prises en charge sur Xbox](http://go.microsoft.com/fwlink/?LinkID=760755).\]

La liste ci-après répertorie certains problèmes connus que vous pourriez rencontrer, mais cette liste n’est pas exhaustive. 

**Vos commentaires nous intéressent**. Par conséquent, n’hésitez pas à nous faire part de vos éventuels problèmes sur le forum [Développement d’applications de plateforme Windows universelle](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop). 

Si vous êtes bloqué, lisez les informations présentées dans cette rubrique, consultez le [Forum aux questions](frequently-asked-questions.md) et utilisez les forums pour demander de l’aide.

 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>Le déploiement à partir de VisualStudio échoue lorsque les paramètres de contrôle parental sont activés

Le lancement de votre application à partir de VisualStudio n’aboutit pas si le contrôle parental est activé dans les paramètres.

Pour contourner ce problème, désactivez ces paramètres temporairement ou procédez comme suit:
1. Déployez votre application sur la console en désactivant les paramètres de contrôle parental.
2. Activez ces paramètres.
3. Lancez votre application à partir de la console.
4. Saisissez un mot de passe ou un codeconfidentiel pour permettre à l’application de se lancer.
5. L’application se lance.
6. Fermez l’application.
7. Effectuez le lancement à partir de Visual Studio à l’aide de la toucheF5; l’application s’ouvre sans afficher d’invite.

À ce stade, l’autorisation est _rémanente_ jusqu’à ce que vous fermiez la session de l’utilisateur, même si vous désinstallez l’application et la réinstallez.
 
Il existe un autre type d’exemption, uniquement disponible pour les comptesenfant. Un compteenfant requiert un parent, qui doit se connecter pour accorder l’autorisation adéquate. Lorsqu’il se connecte, le parent peut choisir d’autoriser systématiquement l’enfant à lancer l’application (paramètre **Toujours**). Cette exemption est stockée dans le cloud et est persistante, même si l’enfant se déconnecte et se reconnecte.

## <a name="storagefilecopyasync-fails-to-copy-encrypted-files-to-unencrypted-destination"></a>StorageFile.CopyAsync ne parvient pas à copier les fichiers chiffrés sur la destination non chiffrée 

Lorsque StorageFile.CopyAsync est utilisé pour copier un fichier chiffré sur une destination non chiffrée, l’appel est mis en échec avec l’exception suivante:

```
System.UnauthorizedAccessException: Access is denied. (Excep_FromHResult 0x80070005)
```

Cela peut affecter les développeurs Xbox souhaitant copier sur un autre emplacement les fichiers déployés de leur package d’application. Pourquoi? En fait, le contenu du package est chiffré sur Xbox en mode commercial, non en mode de développement. Par conséquent, l’application peut fonctionner normalement durant les étapes de développement et de test, puis être mise en échec une fois publiée et installée sur une console Xbox commerciale.
 

## <a name="blocked-networking-ports-on-xbox-one"></a>Ports réseau bloqués sur Xbox One

Les applications de plateforme Windows universelle (UWP) sur les appareils XboxOne ne sont pas autorisées à établir une liaison aux ports dans la plage [57344, 65535]&nbsp;(numéros de port inclus). Même si la liaison à ces ports semble réussir au moment de l’exécution, le trafic réseau peut être annulé sans avertissement avant d’atteindre votre application. Votre application doit si possible établir une liaison au port 0, ce qui permet au système de sélectionner le port local. Si vous avez besoin d’utiliser un port spécifique, le numéro de port doit être dans la plage [1025, 49151]. Vous devez vérifier dans le Registre IANA et éviter les conflits. Pour plus d’informations, voir le [Registre des noms de services et des numéros de ports des protocoles de transport](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml).

## <a name="uwp-api-coverage"></a>Couverture des API UWP

Les APIUWP ne sont pas toutes prises en charge sur Xbox. Pour obtenir la liste des API dont nous savons qu’elles ne fonctionnent pas, voir [Fonctionnalités UWP qui ne sont pas encore prises en charge sur Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755). Si vous rencontrez des problèmes avec d’autres API, signalez-les sur les forums. 


## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>Avertissement de sécurité déclenché par l’accès à WDP

Vous recevrez un avertissement concernant le certificat fourni, semblable à la capture d’écran ci-dessous, car le certificat de sécurité signé par votre console XboxOne n’est pas considéré comme un éditeur approuvé bien connu. Pour accéder à Windows Device Portal, cliquez sur **Poursuivre sur ce site web**.

![Avertissement concernant le certificat de sécurité d’un site web](images/security_cert_warning.jpg)


## <a name="knownfoldersmediaserverdevices-caveat-on-xbox"></a>Risque KnownFolders.MediaServerDevices sur Xbox

Sur le Bureau, les serveurs multimédias sont associés au PC, et le service d’association de périphérique identifie en temps réel les serveurs en ligne, afin qu’une requête initiale de système de fichiers puisse immédiatement renvoyer une liste des serveurs associés.

Xbox ne présente aucune interface utilisateur permettant d’ajouter ou de supprimer des serveurs. En conséquence, la requête initiale de système de fichiers renvoie toujours une réponse vide. Vous devez créer une requête, vous abonner à l’événement ContentsChanged, et actualiser la requête à chaque fois que vous recevez une notification. Les serveurs apparaîtront petit à petit; la plupart d’entre eux seront découverts dans un intervalle de 3secondes.

Exemple de code simple:

```
namespace TestDNLA {

    public sealed partial class MainPage : Page {
        public MainPage() {
            this.InitializeComponent();
        }

        private async void FindFiles_Click(object sender, RoutedEventArgs e) {
            try {
                StorageFolder library = KnownFolders.MediaServerDevices;
                var folderQuery = library.CreateFolderQuery();
                folderQuery.ContentsChanged += FolderQuery_ContentsChanged;
                IReadOnlyList<StorageFolder> rootFolders = await folderQuery.GetFoldersAsync();
                if (rootFolders.Count == 0) {
                    Debug.WriteLine("No Folders found");
                } else {
                    Debug.WriteLine("Folders found");
                }
            } catch (Exception ex) {
                Debug.WriteLine("Error: " + ex.Message);
            } finally {
                Debug.WriteLine("Done");
            }
        }

        private async void FolderQuery_ContentsChanged(Windows.Storage.Search.IStorageQueryResultBase sender, object args) {
            Debug.WriteLine("Folder added " + sender.Folder.Name);
            IReadOnlyList<StorageFolder> topLevelFolders = await sender.Folder.GetFoldersAsync();
            foreach (StorageFolder topLevelFolder in topLevelFolders) {
                Debug.WriteLine(topLevelFolder.Name);
            }
        }
    }
}
```

## <a name="see-also"></a>Voir également
- [Forum Aux Questions](frequently-asked-questions.md)
- [UWP sur XboxOne](index.md)
