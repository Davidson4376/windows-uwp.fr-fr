---
author: jnHs
Description: You can create Store listings for your apps without using Partner Center by exporting your listings in a .csv file, entering your info and assets, and then importing the updated file.
title: Importer et exporter des descriptions dans le Store
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows10, uwp, importer des descriptions dans le store, exporter des descriptions du store, importer exporter, description dans le store csv
ms.localizationpriority: medium
ms.openlocfilehash: 9df66baa1b5771055894b2e3accc0c8a9dff8a60
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6047637"
---
# <a name="import-and-export-store-listings"></a>Importer et exporter des descriptions dans le Store

Au lieu d' [entrer des informations de votre description dans le Windows Store directement dans l’espace partenaires](create-app-store-listings.md), vous avez la possibilité d’ajouter ou de mettre à jour les infos en exportant vos descriptions dans un fichier .csv, entrez vos informations et composants, puis en important le fichier mis à jour. Vous pouvez utiliser cette méthode pour créer des descriptions à partir de zéro ou pour mettre à jour des descriptions déjà créées.

Cette option est particulièrement utile si vous souhaitez créer ou mettre à jour des descriptions dans le Store en plusieurs langues pour votre produit puisque vous pouvez copier/coller les mêmes informations dans plusieurs champs et facilement apporter des modifications qui doivent s’appliquer à des langues spécifiques. Toutefois, vous ne pouvez pas utiliser cette méthode pour créer ou mettre à jour des [descriptions spécifiques à la plateforme](create-platform-specific-store-listings.md) pour les applications publiée précédemment qui prennent en charge des versions antérieures du système d’exploitation. 

> [!TIP]
> Vous pouvez également utiliser cette fonctionnalité pour importer et exporter les détails d’une description dans le WindowsStore concernant une extension. Pour les extensions, le processus fonctionne de la même façon, à l’exception du fait que [seuls les champs applicables aux extensions](#add-ons) sont inclus.

N’oubliez pas que vous pouvez toujours créer ou mettre à jour des descriptions directement dans l’espace partenaires (même si vous avez déjà utilisé la méthode d’importation/exportation). Mise à jour directement dans l’espace partenaires peut être plus facile lorsque vous rendez simplement une simple modification, mais vous pouvez utiliser ces deux méthodes à tout moment.

## <a name="export-listings"></a>Exporter des descriptions

Dans la page de vue d’ensemble de la soumission d’une application, cliquez sur **Export listing** (dans la section **Store listings**) pour générer un fichier.csv encodé en UTF-8. Enregistrez ce fichier sur votre ordinateur.

Vous pouvez modifier ce fichier à l’aide de MicrosoftExcel ou d’un autre éditeur. Notez que les versions Office365 d’Excel vous permettent d’enregistrer un fichier.csv au format **CSV UTF-8 (délimité par des virgules) (*.csv)**, mais que les autres versions risquent de ne pas prendre en charge cette fonctionnalité. Pour plus d’informations sur les versions d’Excel qui prennent en charge cette fonctionnalité, consultez le [bulletin relatif aux nouveautés d’Excel2016](https://support.office.com/en-us/article/What-s-new-in-Excel-2016-for-Windows-5fdb9208-ff33-45b6-9e08-1f5cdb3a6c73); pour plus d’informations sur l’encodage au format UTF-8 dans différents éditeurs, [cliquez ici](https://help.surveygizmo.com/help/encode-an-excel-file-to-utf-8-or-utf-16).
      
Si vous n’avez pas encore créé de descriptions pour votre produit, le fichier.csv que vous avez exporté ne contient aucune donnée personnalisée. Ce fichier comporte des colonnes **Champ**, **ID**, **Type** et **valeur par défaut**, ainsi que des lignes qui correspondent à chaque élément pouvant apparaître dans une description dans le WindowsStore.

Si vous avez déjà créé des descriptions (ou chargé des packages), ce fichier comporte des colonnes libellées avec les codes de paramètres régionaux de langue qui correspondent à la langue de chaque description créée (ou à celle que nous avons détectée dans vos packages), ainsi que toutes les informations de description que vous avez précédemment fournies.
     
Voici une vue d’ensemble du contenu de chacune des colonnes du fichier.csv exporté:
- La colonne **Champ** contient un nom associé à chaque partie d’une description dans le WindowsStore. Ces caractères correspondent aux éléments que vous pouvez fournir lorsque vous créez des descriptions du Windows Store dans l’espace partenaires, bien que certains noms sont légèrement différents. Dans le cas des éléments pour lesquels vous pouvez entrer plusieurs types d’éléments, le fichier affiche le nombre maximal de lignes autorisé. Par exemple, pour **Fonctionnalités de l’application**, le fichier affiche les lignes **Fonctionnalité1**, **Fonctionnalité2**, etc., jusqu’à **Fonctionnalité20** (puisque vous pouvez spécifier jusqu’à 20fonctionnalités d’application).
- La colonne **ID** contient une valeur que l’espace partenaires associe à chaque champ. 
- La colonne **Type** fournit des indications générales sur le type d’informations à fournir pour chaque champ, par exemple, le **texte** ou un **chemin d’accès relatif (ou URL vers le fichier dans l’espace partenaires)**. 
- La colonne **valeur par défaut** (ainsi que toute colonne libellée avec des codes de paramètres régionaux de langue) représente le texte ou les composants associés à chaque partie de la description dans le WindowsStore. Vous pouvez modifier les champs de ces colonnes pour mettre à jour vos descriptions dans le WindowsStore.

>[!IMPORTANT]
> Ne modifiez pas les informations des colonnes **Champ**, **ID** ou **Type**. Les informations figurant dans ces colonnes doivent rester inchangées pour que votre fichier importé soit correctement traité.

## <a name="update-listing-info"></a>Mettre à jour les informations de description

Une fois que vous avez exporté vos descriptions et enregistré votre fichier.csv, vous pouvez modifier vos informations de description directement dans le fichier.csv. 

Parallèlement à la colonne **valeur par défaut**, chaque langue pour laquelle vous avez créé une description comporte sa propre colonne. Les modifications que vous apportez dans une colonne s’appliqueront à votre description dans cette langue. Vous pouvez créer des descriptions dans d’autres langues en ajoutant le code de paramètres régionaux de langue dans la colonne vide suivante de la ligne supérieure. Pour découvrir la liste des codes de paramètres régionaux de langue, consultez l’article [Langues prises en charge](supported-languages.md).

Vous pouvez utiliser la colonne **valeur par défaut** pour entrer les informations que vous souhaitez partager entre toutes les descriptions de votre application. Si le champ d’une langue donnée est laissé vide, les informations de la colonne de valeur par défaut seront utilisées pour cette langue. Vous pouvez remplacer le champ d’une langue spécifique en entrant d’autres informations pour cette langue.

La plupart des champs de description dans le WindowsStore sont facultatifs. Chaque description dans le WindowsStore requiert une **Description** et une capture d’écran; dans le cas des langues auxquelles aucun package n’est associé, vous devez également renseigner le champ **Title** afin d’indiquer vos noms d’application réservés qui doivent être utilisés pour cette description. Vous pouvez laisser tous les autres champs vides si vous ne souhaitez pas les inclure dans votre description. Souvenez-vous que si vous ne renseignez pas le champ d’une langue spécifique, nous vérifierons si des informations sont spécifiées pour ce champ dans la colonne de valeur par défaut. Si tel est le cas, ces informations seront utilisées. 

Considérons l’exemple suivant: 

![Exemple de description exportée](images/listingimport.png)
     
- Dans les descriptions en-us et fr-fr, le texte «Description par défaut» est utilisé pour le champ **Description**. En revanche, dans la description es-es, le champ **Description** utilise le texte «Description en espagnol». 
- Dans le cas du champ **ReleaseNotes**, le texte «Notes de publication en anglais» est utilisé pour la description en-us, tandis que le texte «Notes de publication en français» est utilisé pour la description fr-fr. Toutefois, aucune note de publication n’apparaît pour la description es-es.

Si vous ne souhaitez pas apporter de modifications à un champ spécifique, vous pouvez supprimer la totalité de la ligne de la feuille de calcul, **à l’exception des lignes relatives aux bandes-annonces et aux miniatures et titres qui leur sont associés**. Hormis pour ces éléments, la suppression d’une ligne n’a aucune incidence sur les données associées à ce champ dans vos descriptions. Ceci vous permet de supprimer les lignes que vous n’envisagez pas de modifier et de vous concentrer sur les champs auxquels vous apportez des modifications.

La suppression des informations dans le champ d’une langue donnée, sans supprimer la totalité de la ligne, fonctionne différemment selon les champs. Dans le cas des champs dont le **Type** est défini sur **Texte**, la suppression des informations dans le champ supprime simplement cette entrée de la description dans cette langue.  Toutefois, la suppression des informations dans le champ d’une image, telle qu’une capture d’écran ou un logo, pas aura aucun effet; l’image précédente sera toujours être utilisé, sauf si vous le supprimer en modifiant directement dans l’espace partenaires. La suppression des informations relatives à un champ de bande-annonce pour suppression de cette bande-annonce à partir de l’espace partenaires, par conséquent, veillez à conserver une copie de tous les fichiers nécessaires avant de faire.

La plupart des champs de vos descriptions exportées, tels que les champs **Description** et **ReleaseNotes** de l’exemple précédent, requièrent une saisie de texte. Pour ces types de champs, il vous suffit d’entrer le texte approprié dans le champ associé à chaque langue. Prenez soin de respecter les restrictions de longueur et les autres exigences propres à chacun des champs. Pour plus d’informations sur ces exigences, consultez l’article [Créer des annonces d’application dans le WindowsStore](create-app-store-listings.md).

La fourniture d’informations pour les champs qui correspondent à des composants, tels que des images et des bandes-annonces, se révèle légèrement plus complexe. Au lieu de **texte**, le **Type** de ces ressources est le **chemin d’accès relatif (ou URL vers le fichier dans l’espace partenaires)**. 
     
Si vous avez déjà chargé des composants pour vos descriptions dans le WindowsStore, ces composants sont représentés par une URL. Ces URL sont réutilisables dans plusieurs descriptions d’un produit, ou même pour différents produits du même compte de développeur; si vous le souhaitez, vous pouvez donc copier ces URL pour les réutiliser dans un autre champ de votre choix.

> [!TIP]
> Pour vérifier le composant auquel correspond une URL donnée, vous pouvez entrer cette URL dans un navigateur afin de visualiser l’image associée (ou de télécharger le fichier vidéo de bande-annonce).  Vous devez être connecté à votre compte espace partenaires afin que cette URL fonctionne.

Si vous souhaitez utiliser un nouveau composant que vous n’avez pas encore ajouté à l’espace partenaires, vous pouvez le faire en important vos descriptions en tant que dossier au lieu d’un fichier .csv unique. Vous devez créer un dossier contenant votre fichier.csv. Ensuite, ajoutez vos images à ce dossier, soit dans le dossier racine, soit dans un sous-dossier. Dans le champ, vous devrez entrer le chemin d’accès complet, y compris le nom du dossier racine.

> [!TIP]
> Pour optimiser l’importation de vos descriptions sous la forme d’un dossier, veillez à utiliser la dernière version de MicrosoftEdge, de Chrome ou de Firefox.

Par exemple, si votre dossier racine est nommé **my_folder** et que vous souhaitez utiliser une image appelée **screenshot1.png** pour **DesktopScreenshot1**, vous pouvez ajouter screenshot1.png à la racine de ce dossier, puis entrer **my_folder/screenshot1.png** dans le champ **DesktopScreenshot1**. En revanche, si vous créez un sous-dossier images dans votre dossier racine, puis que vous placez screenshot1.jpg dans ce sous-dossier, vous devrez entrer le chemin d’accès **my_folder/images/screenshot1.png**. Notez qu’une fois que vous importez vos annonces à l’aide d’un dossier, chemins d’accès à vos images seront convertis aux URL vers les fichiers dans l’espace partenaires la prochaine fois que vous exporterez vos descriptions. Vous pouvez copier et coller ces URL pour les réutiliser (par exemple, si vous souhaitez utiliser les mêmes composants dans les descriptions en plusieurs langues). 

> [!IMPORTANT]
> Si votre description exportée inclut des bandes-annonces, n’oubliez pas que la suppression de l’URL de la bande-annonce ou son image miniature de votre fichier .csv supprimera complètement le fichier supprimé de l’espace partenaires, et vous ne serez n’est plus en mesure d’accéder (sauf s’il est également utilisé dans ano frais de description où il n’a pas encore été supprimé). 

## <a name="import-listings"></a>Importer des descriptions

Une fois que vous avez entré toutes vos modifications dans le fichier.csv (et que vous y avez inclus tous les composants que vous souhaitez charger), vous devez enregistrer ce fichier avant de le charger. Si votre version de MicrosoftExcel prend en charge l’encodage UTF-8, veillez à sélectionner **Enregistrer sous** et à utiliser le format **CSV UTF-8 (délimité par des virgules) (*.csv)**. Si vous utilisez un autre éditeur pour visualiser et modifier votre fichier.csv, assurez-vous que ce fichier est encodé en UTF-8 avant de le charger.

Une fois que vous êtes prêt à charger le fichier.csv mis à jour et à importer vos données de description, sélectionnez **Import listings** sur la page de vue d’ensemble de votre soumission. Si vous importez uniquement un fichier.csv, choisissez **Importer au formatCSV**, accédez à votre fichier, puis cliquez sur **Ouvrir**. Si vous importez un dossier contenant des fichiers images, choisissez Import folder, accédez à votre dossier, puis cliquez sur **Sélectionner un dossier**. Assurez-vous que votre dossier ne contient qu’un seul fichier.csv, ainsi que les composants dont vous effectuez le chargement. 

Pendant que nous traitons le fichier.csv que vous importez, vous voyez apparaître une barre de progression présentant l’état de l’importation et de la validation. Cette opération peut prendre un certain temps, en particulier si vous importez un grand nombre de descriptions et/ou de fichiers images. 

Si nous détectons des problèmes, vous obtiendrez un message vous invitant à effectuer les modifications nécessaires, puis à relancer l’opération. Pour visualiser les champs incorrects et en connaître la raison, sélectionnez le lien **Afficher les erreurs**. Vous devrez corriger ces erreurs dans votre fichier.csv (ou remplacer tous les composants non valides), puis réimporter vos descriptions.

> [!TIP]
> Par la suite, vous pourrez reconsulter ces informations par le biais du lien **View errors for last import**.

Aucune des informations de votre fichier .csv ne sera enregistrée dans l’espace partenaires jusqu'à ce que toutes les erreurs dans votre fichier ont été résolus, même pour les champs dépourvus d’erreurs. Une fois que vous avez importé un fichier .csv exempt d’erreurs, les informations de description que vous avez fournies sont enregistrées dans l’espace partenaires et seront utilisée pour cette soumission.

Vous pouvez continuer à mettre à jour vos descriptions en important un autre fichier .csv mis à jour, soit en apportant des modifications directement dans l’espace partenaires.

## <a name="add-ons"></a>Extensions

Pour les extensions, importer et exporter des descriptions du Windows Store utilise le même processus décrit ci-dessus, à ceci près que vous ne verrez les trois champs applicables aux [descriptions de module complémentaire](create-add-on-store-listings.md): **Description**, le **titre**et **StoreLogo300x300** () dénommé sous forme **d’icône** dans la page de description du Windows Store dans l’espace partenaires). Le champ **Title** est obligatoire, tandis que les deux autres champs sont facultatifs.

Notez que les descriptions de chaque extension de votre application dans le WindowsStore doivent être importées et exportées séparément par l’intermédiaire de la page de vue d’ensemble de la soumission de ces extensions.


