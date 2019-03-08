---
Description: Vous pouvez créer des listes de Store pour vos applications sans l’espace partenaires en exportant vos annonces dans un fichier .csv, entrez vos informations et vos ressources et ensuite l’importation du fichier mis à jour.
title: Importer et exporter des descriptions dans le Store
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, importer des descriptions dans le store, exporter des descriptions du store, importer exporter, description dans le store csv
ms.localizationpriority: medium
ms.openlocfilehash: 5630a9019aa11b87f06744e03ae74ec38c792d41
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636434"
---
# <a name="import-and-export-store-listings"></a>Importer et exporter des descriptions dans le Store

Au lieu de [saisie des informations pour vos annonces Store directement dans l’espace partenaires](create-app-store-listings.md), vous avez la possibilité d’ajouter ou mettre à jour les informations en exportant vos annonces dans un fichier .csv, entrez vos informations et vos ressources et ensuite l’importation du fichier mis à jour. Vous pouvez utiliser cette méthode pour créer des descriptions à partir de zéro ou pour mettre à jour des descriptions déjà créées.

Cette option est particulièrement utile si vous souhaitez créer ou mettre à jour des descriptions dans le Store en plusieurs langues pour votre produit puisque vous pouvez copier/coller les mêmes informations dans plusieurs champs et facilement apporter des modifications qui doivent s’appliquer à des langues spécifiques. Toutefois, vous ne pouvez pas utiliser cette méthode pour créer ou mettre à jour [annonces de Store spécifique à la plateforme](create-platform-specific-store-listings.md) pour les applications déjà publiée qui prennent en charge les versions antérieures du système d’exploitation. 

> [!TIP]
> Vous pouvez également utiliser cette fonctionnalité pour importer et exporter les détails d’une description dans le Windows Store concernant une extension. Pour les extensions, le processus fonctionne de la même façon, à l’exception du fait que [seuls les champs applicables aux extensions](#add-ons) sont inclus.

N’oubliez pas que vous pouvez toujours créer ou mettre à jour les listes directement dans l’espace partenaires (même si vous avez déjà utilisé la méthode d’importation/exportation). La mise à jour directement dans l’espace partenaires peut être plus facile lorsque vous effectuez simplement une modification simple, mais vous pouvez utiliser soit la méthode à tout moment.

## <a name="export-listings"></a>Exporter des descriptions

Dans la page de vue d’ensemble de la soumission d’une application, cliquez sur **Export listing** (dans la section **Store listings**) pour générer un fichier .csv encodé en UTF-8. Enregistrez ce fichier sur votre ordinateur.

Vous pouvez modifier ce fichier à l’aide de Microsoft Excel ou d’un autre éditeur. Notez que les versions Office 365 d’Excel vous permettent d’enregistrer un fichier .csv au format **CSV UTF-8 (délimité par des virgules) (*.csv)**, mais que les autres versions risquent de ne pas prendre en charge cette fonctionnalité. Pour plus d’informations sur les versions d’Excel qui prennent en charge cette fonctionnalité, consultez le [bulletin relatif aux nouveautés d’Excel 2016](https://support.office.com/en-us/article/What-s-new-in-Excel-2016-for-Windows-5fdb9208-ff33-45b6-9e08-1f5cdb3a6c73) ; pour plus d’informations sur l’encodage au format UTF-8 dans différents éditeurs, [cliquez ici](https://help.surveygizmo.com/help/encode-an-excel-file-to-utf-8-or-utf-16).
      
Si vous n’avez pas encore créé de descriptions pour votre produit, le fichier .csv que vous avez exporté ne contient aucune donnée personnalisée. Ce fichier comporte des colonnes **Champ**, **ID**, **Type** et **valeur par défaut**, ainsi que des lignes qui correspondent à chaque élément pouvant apparaître dans une description dans le Windows Store.

Si vous avez déjà créé des descriptions (ou chargé des packages), ce fichier comporte des colonnes libellées avec les codes de paramètres régionaux de langue qui correspondent à la langue de chaque description créée (ou à celle que nous avons détectée dans vos packages), ainsi que toutes les informations de description que vous avez précédemment fournies.
     
Voici une vue d’ensemble du contenu de chacune des colonnes du fichier .csv exporté :
- La colonne **Champ** contient un nom associé à chaque partie d’une description dans le Windows Store. Ils correspondent aux mêmes éléments que vous pouvez fournir lors de la création de listes de Store dans Partner Center, bien que certains noms sont légèrement différentes. Dans le cas des éléments pour lesquels vous pouvez entrer plusieurs types d’éléments, le fichier affiche le nombre maximal de lignes autorisé. Par exemple, pour **Fonctionnalités de l’application**, le fichier affiche les lignes **Fonctionnalité1**, **Fonctionnalité2**, etc., jusqu’à **Fonctionnalité20** (puisque vous pouvez spécifier jusqu’à 20 fonctionnalités d’application).
- Le **ID** colonne contient un nombre qui associe des partenaires avec chaque champ. 
- Le **Type** colonne fournit des indications générales sur le type d’informations à fournir pour ce champ, tel que **texte** ou **chemin d’accès relatif (ou URL du fichier dans espace partenaires)**. 
- La colonne **valeur par défaut** (ainsi que toute colonne libellée avec des codes de paramètres régionaux de langue) représente le texte ou les composants associés à chaque partie de la description dans le Windows Store. Vous pouvez modifier les champs de ces colonnes pour mettre à jour vos descriptions dans le Windows Store.

>[!IMPORTANT]
> Ne modifiez pas les informations des colonnes **Champ**, **ID** ou **Type**. Les informations figurant dans ces colonnes doivent rester inchangées pour que votre fichier importé soit correctement traité.

## <a name="update-listing-info"></a>Mettre à jour les informations de description

Une fois que vous avez exporté vos descriptions et enregistré votre fichier .csv, vous pouvez modifier vos informations de description directement dans le fichier .csv. 

Parallèlement à la colonne **valeur par défaut**, chaque langue pour laquelle vous avez créé une description comporte sa propre colonne. Les modifications que vous apportez dans une colonne s’appliqueront à votre description dans cette langue. Vous pouvez créer des descriptions dans d’autres langues en ajoutant le code de paramètres régionaux de langue dans la colonne vide suivante de la ligne supérieure. Pour découvrir la liste des codes de paramètres régionaux de langue, consultez l’article [Langues prises en charge](supported-languages.md).

Vous pouvez utiliser la colonne **valeur par défaut** pour entrer les informations que vous souhaitez partager entre toutes les descriptions de votre application. Si le champ d’une langue donnée est laissé vide, les informations de la colonne de valeur par défaut seront utilisées pour cette langue. Vous pouvez remplacer le champ d’une langue spécifique en entrant d’autres informations pour cette langue.

La plupart des champs de description dans le Windows Store sont facultatifs. Chaque description dans le Windows Store requiert une **Description** et une capture d’écran ; dans le cas des langues auxquelles aucun package n’est associé, vous devez également renseigner le champ **Title** afin d’indiquer vos noms d’application réservés qui doivent être utilisés pour cette description. Vous pouvez laisser tous les autres champs vides si vous ne souhaitez pas les inclure dans votre description. Souvenez-vous que si vous ne renseignez pas le champ d’une langue spécifique, nous vérifierons si des informations sont spécifiées pour ce champ dans la colonne de valeur par défaut. Si tel est le cas, ces informations seront utilisées. 

Considérons l’exemple suivant : 

![Exemple de description exportée](images/listingimport.png)
     
- Dans les descriptions en-us et fr-fr, le texte « Description par défaut » est utilisé pour le champ **Description**. En revanche, dans la description es-es, le champ **Description** utilise le texte « Description en espagnol ». 
- Dans le cas du champ **ReleaseNotes**, le texte « Notes de publication en anglais » est utilisé pour la description en-us, tandis que le texte « Notes de publication en français » est utilisé pour la description fr-fr. Toutefois, aucune note de publication n’apparaît pour la description es-es.

Si vous ne souhaitez pas apporter de modifications à un champ spécifique, vous pouvez supprimer la totalité de la ligne de la feuille de calcul, **à l’exception des lignes relatives aux bandes-annonces et aux miniatures et titres qui leur sont associés**. Hormis pour ces éléments, la suppression d’une ligne n’a aucune incidence sur les données associées à ce champ dans vos descriptions. Ceci vous permet de supprimer les lignes que vous n’envisagez pas de modifier et de vous concentrer sur les champs auxquels vous apportez des modifications.

La suppression des informations dans le champ d’une langue donnée, sans supprimer la totalité de la ligne, fonctionne différemment selon les champs. Dans le cas des champs dont le **Type** est défini sur **Texte**, la suppression des informations dans le champ supprime simplement cette entrée de la description dans cette langue.  Toutefois, la suppression les informations dans un champ pour une image, tel qu’une capture d’écran ou un logo, pas ont d’effet ; l’image précédente sera toujours être utilisé, sauf si vous la supprimez en modifiant directement dans l’espace partenaires. Supprimer les informations pour un champ de code de fin pour réellement supprimer ce code de fin de partenaires, par conséquent, veillez à que disposer d’une copie de tous les fichiers nécessaires avant de faire.

La plupart des champs de vos descriptions exportées, tels que les champs **Description** et **ReleaseNotes** de l’exemple précédent, requièrent une saisie de texte. Pour ces types de champs, il vous suffit d’entrer le texte approprié dans le champ associé à chaque langue. Prenez soin de respecter les restrictions de longueur et les autres exigences propres à chacun des champs. Pour plus d’informations sur ces exigences, consultez l’article [Créer des annonces d’application dans le Windows Store](create-app-store-listings.md).

La fourniture d’informations pour les champs qui correspondent à des composants, tels que des images et des bandes-annonces, se révèle légèrement plus complexe. Au lieu de **texte**, le **Type** pour ces ressources est **chemin d’accès relatif (ou URL du fichier dans espace partenaires)**. 
     
Si vous avez déjà chargé des composants pour vos descriptions dans le Windows Store, ces composants sont représentés par une URL. Ces URL sont réutilisables dans plusieurs descriptions d’un produit, ou même pour différents produits du même compte de développeur ; si vous le souhaitez, vous pouvez donc copier ces URL pour les réutiliser dans un autre champ de votre choix.

> [!TIP]
> Pour vérifier le composant auquel correspond une URL donnée, vous pouvez entrer cette URL dans un navigateur afin de visualiser l’image associée (ou de télécharger le fichier vidéo de bande-annonce).  Vous devez être connecté à votre compte espace partenaires afin que cette URL fonctionne.

Si vous souhaitez utiliser un nouvel élément multimédia que vous n’avez pas précédemment ajoutés à Partner Center, vous pouvez le faire en important vos listes en tant que dossier, plutôt que comme un fichier .csv. Vous devez créer un dossier contenant votre fichier .csv. Ensuite, ajoutez vos images à ce dossier, soit dans le dossier racine, soit dans un sous-dossier. Dans le champ, vous devrez entrer le chemin d’accès complet, y compris le nom du dossier racine.

> [!TIP]
> Pour optimiser l’importation de vos descriptions sous la forme d’un dossier, veillez à utiliser la dernière version de Microsoft Edge, de Chrome ou de Firefox.

Par exemple, si votre dossier racine est nommé **my_folder** et que vous souhaitez utiliser une image appelée **screenshot1.png** pour **DesktopScreenshot1**, vous pouvez ajouter screenshot1.png à la racine de ce dossier, puis entrer **my_folder/screenshot1.png** dans le champ **DesktopScreenshot1**. En revanche, si vous créez un sous-dossier images dans votre dossier racine, puis que vous placez screenshot1.jpg dans ce sous-dossier, vous devrez entrer le chemin d’accès **my_folder/images/screenshot1.png**. Notez qu’après avoir importé vos annonces à l’aide d’un dossier, les chemins d’accès à vos images seront en URL aux fichiers dans l’espace partenaires la prochaine fois que vous exportez vos annonces. Vous pouvez copier et coller ces URL pour les réutiliser (par exemple, si vous souhaitez utiliser les mêmes composants dans les descriptions en plusieurs langues). 

> [!IMPORTANT]
> Si votre annonce exporté inclut des codes de fin, n’oubliez pas que la suppression de l’URL pour le code de fin ou la vignette correspondante à partir de votre fichier .csv supprimera complètement le fichier supprimé à partir du centre de partenaires, et ne plus pouvoir accéder (sauf si elle est également utilisée dans ano à exis liste où il n’a pas été supprimé). 

## <a name="import-listings"></a>Importer des descriptions

Une fois que vous avez entré toutes vos modifications dans le fichier .csv (et que vous y avez inclus tous les composants que vous souhaitez charger), vous devez enregistrer ce fichier avant de le charger. Si votre version de Microsoft Excel prend en charge l’encodage UTF-8, veillez à sélectionner **Enregistrer sous** et à utiliser le format **CSV UTF-8 (délimité par des virgules) (*.csv)**. Si vous utilisez un autre éditeur pour visualiser et modifier votre fichier .csv, assurez-vous que ce fichier est encodé en UTF-8 avant de le charger.

Une fois que vous êtes prêt à charger le fichier .csv mis à jour et à importer vos données de description, sélectionnez **Import listings** sur la page de vue d’ensemble de votre soumission. Si vous importez uniquement un fichier .csv, choisissez **Importer au format CSV**, accédez à votre fichier, puis cliquez sur **Ouvrir**. Si vous importez un dossier contenant des fichiers images, choisissez Import folder, accédez à votre dossier, puis cliquez sur **Sélectionner un dossier**. Assurez-vous que votre dossier ne contient qu’un seul fichier .csv, ainsi que les composants dont vous effectuez le chargement. 

Pendant que nous traitons le fichier .csv que vous importez, vous voyez apparaître une barre de progression présentant l’état de l’importation et de la validation. Cette opération peut prendre un certain temps, en particulier si vous importez un grand nombre de descriptions et/ou de fichiers images. 

Si nous détectons des problèmes, vous obtiendrez un message vous invitant à effectuer les modifications nécessaires, puis à relancer l’opération. Pour visualiser les champs incorrects et en connaître la raison, sélectionnez le lien **Afficher les erreurs**. Vous devrez corriger ces erreurs dans votre fichier .csv (ou remplacer tous les composants non valides), puis réimporter vos descriptions.

> [!TIP]
> Par la suite, vous pourrez reconsulter ces informations par le biais du lien **View errors for last import**.

Aucune des informations à partir de votre fichier .csv sera enregistré dans partenaires jusqu'à ce que toutes les erreurs dans votre fichier ont été résolus, même pour les champs sans erreurs. Une fois que vous avez importé un fichier .csv qui comporte pas d’erreurs, les informations de liste que vous avez fourni seront enregistrée dans partenaires et seront utilisée pour cette soumission.

Vous pouvez continuer à mettre à jour vos listes en important un autre fichier .csv mis à jour, ou en apportant des modifications directement dans l’espace partenaires.

## <a name="add-ons"></a>Extensions

Pour les modules complémentaires, importation et exportation de listes de Store utilise le même processus décrit ci-dessus, à ceci près que vous voyez uniquement les trois champs pertinents pour [listes de module complémentaire Store](create-add-on-store-listings.md): **Description**, **titre**, et **StoreLogo300x300** (appelé **icône** dans la page de liste de Store dans partenaires). Le champ **Title** est obligatoire, tandis que les deux autres champs sont facultatifs.

Notez que les descriptions de chaque extension de votre application dans le Windows Store doivent être importées et exportées séparément par l’intermédiaire de la page de vue d’ensemble de la soumission de ces extensions.


