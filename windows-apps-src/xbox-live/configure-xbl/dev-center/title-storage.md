---
title: Configuration du stockage dans le centre de partenaires du titre
description: Découvrez comment configurer le stockage de titre dans Partner Center
ms.date: 04/24/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, jeux, uwp, windows 10, Xbox une, de stockage de titre, de partenaires
ms.openlocfilehash: d32f9f2f4e003db50ad560acc513511a850d43b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612094"
---
# <a name="configure-storage-for-you-title-in-partner-center"></a>Configurer le stockage pour vous titre dans le centre de partenaires

Xbox Live vous permet d’enregistrer les données associées à votre jeu dans le cloud via le service de stockage de titre. La page de configuration de stockage de titre vous permet de déterminer quels types de services de stockage cloud votre jeu sera autoriser, ainsi que télécharger des fichiers à utiliser pour le stockage Global.

Vous pouvez trouver la page de configuration de stockage de titre Xbox Live en accédant à [partenaire](https://partner.microsoft.com/dashboard), en choisissant votre application à partir de **vue d’ensemble** ou **produits**, en ouvrant le  **Services** liste déroulante et en sélectionnant **Xbox Live**. Les développeurs dans le programme Creators devrez cliquer sur **afficher les options** dans le **enregistre et stockage Cloud** section de leur page de configuration pour afficher les options de configuration du stockage de titre. Ceux avec l’ensemble complet des fonctionnalités Xbox Live disponibles vont devoir trouver la **titre stockage** lien pour accéder à la page de configuration du stockage de titre.

Configuration du stockage titre comporte deux sections principales. La section de paramètres de stockage de titre et la section de gestion de fichier de stockage global.

## <a name="section-1-title-storage-settings"></a>Section 1 : Paramètres de stockage du titre

![Capture d’écran des paramètres de stockage du titre](../../images/dev-center/title-storage/title-storage-settings.JPG)

Dans cette section vous pouvez activer l’une des quatre types de stockage en cochant la case la **Active** colonne. Une fois le choix vous titre 's des types de stockage, vous pouvez choisir si les lire les données est limitée à l’acteur qui le possède, en cliquant sur la ligne de type stockage **propriétaire** uniquement case d’option ou partagés publiquement, en cliquant sur le **Tout le monde** case d’option. Si vous sélectionnez **propriétaire uniquement** pour une donnée **Type de stockage** ensuite les données du stockage de titre de ce type pourra uniquement être lire par le lecteur qui a généré ces données. Si vous sélectionnez **tout le monde** pour une donnée **Type de stockage** alors les données du stockage de titre de ce type sera accessible en lecture à tous les joueurs. Écrivant ou en modifiant les données enregistrées est uniquement disponible pour l’utilisateur qui a généré dans tous les cas.

## <a name="storage-types"></a>Types de stockage

Il existe quatre types de stockage qui peuvent être activées dans la page de configuration du stockage de titre. Vous trouverez une description de chaque type de stockage en plaçant le curseur sur l’icône d’informations en regard du nom de chaque type stockage, ou en lisant le tableau ci-dessous.

|Type de stockage |Description |Exemple d’utilisation  |
|---------|---------|---------|
|Globale             |Chargé de données pour les partenaires qui peuvent être lues par n’importe quel appareil et est accessible à tous les utilisateurs. Peut uniquement être écrite pour par les téléchargements de développeur aux partenaires. | Publier des mises à jour à tous les utilisateurs via l’échange de news dans le jeu.     |
|Stockage connecté  |Permet d’arrière-plan la synchronisation des données de jeu sur XboxOne et jeux de Windows 10. Un jeu à tolérance de pannes robustes enregistrement du service. Peuvent être lues par n’importe quel appareil, peuvent être écrites dans par les appareils Xbox One et Windows 10    | Enregistrer des fichiers pour un utilisateur individuel permettre la lecture sur une console distincte.         |
|Universel          |Réseau de stockage d’objets blob accessible que donne en lecture/écriture à n’importe quel appareil qui n’est pas une Xbox 360 ou sur Windows Phone. Peut être lu par les appareils IOS et Android.      | Enregistrer la lecture ou autres statistiques soient accessibles à partir de plusieurs appareils Windows.        |
|approuvé            |Stockage de blob accessible de réseau qui peut uniquement être écrite en Xbox One, Xbox 360 et Windows Phone. Peuvent être lues par n’importe quel appareil. Peuvent être lues par IOS et Android.     | stocker le classement d’un joueur en mode multijoueur.        |

## <a name="section-2-global-storage-file-management"></a>Section 2 : Gestion de fichiers de stockage globale

![capture d’écran de gestion de fichier stockage global](../../images/dev-center/title-storage/global-storage-file-management.JPG)

Pour afficher les options de la gestion de fichiers de stockage Global complète, vous devez cliquer sur le **afficher les options** liste déroulante. Dans cette section, vous pouvez ajouter les fichiers et dossiers qui seront accessibles si le **Global** type de stockage est défini comme actif dans les paramètres de stockage de titre. Votre jeu sera doivent être publiés pour le test afin d’ajouter des fichiers dans cette section. Vous pouvez voir un avertissement en haut de la page de configuration du stockage de titre si votre jeu n’est pas correctement publié pour le test.

## <a name="manage-global-storage-files"></a>Gérer les fichiers de stockage Global

Gestion des fichiers de stockage global vous permet de charger et télécharger des fichiers à utiliser pour le stockage global. Ces fichiers sont potentiellement accessible par toute personne possède votre titre et sont destinés à être partagés entre tous les joueurs jouent à votre jeu. Pour afficher les options du fichier de stockage global, vous devez cliquer sur le **afficher les options** liste déroulante à côté du titre de sections. Pour ajouter votre premier fichier, cliquez sur le **+ nouveau...**  lien. Vous aurez ensuite la possibilité d’ajouter un nouveau fichier ou dossier à vos fichiers de stockage global.

### <a name="new-folders"></a>Nouveaux dossiers

Lorsque Ajouter un nouveau dossier dans votre stockage global fichiers que vous devez simplement nommez le dossier et cliquez sur le **créer** bouton. Votre nouveau dossier s’affiche dans la table de l’Explorateur de fichiers.

![ajouter la boîte de dialogue dossier](../../images/dev-center/title-storage/add-folder-global-storage-filled.JPG)

Pour ajouter des fichiers dans votre dossier vous devez les télécharger vers le dossier directement en faisant glisser les dossiers **Actions** bouton : «**...** », puis en sélectionnant **charger des fichiers**. Vous ne pouvez pas glisser -déplacer les fichiers dans des dossiers au sein de la table de l’Explorateur de fichiers. À l’aide de la **créer le dossier** action dans le **Actions** menu d’un dossier créera un dossier enfant. En choisissant le **supprimer** action dans le **Actions** menu d’un dossier supprimera le dossier et tout son contenu.

### <a name="new-files"></a>Nouveaux fichiers

Lorsque vous ajoutez un nouveau fichier dans le stockage global vous être invité à télécharger un fichier à partir de l’Explorateur de fichiers de votre ordinateur et ensuite invité à choisir parmi les trois types de fichiers, binaire, configuration et JSON. En plus de pouvoir charger un fichier avec le **+ nouveau...**  bouton, vous pouvez également faire glisser et déposer les fichiers à partir de votre ordinateur à la table de l’Explorateur de fichiers.

> [!WARNING]
> Vous ne peut pas faire glisser les dossiers dans la table de l’Explorateur de fichiers, d’effectuer cette opération entraîne dans le dossier qui est traité comme un fichier et ne fonctionnera pas comme prévu.

Actions de gestion de fichiers : ![gif de gestion de fichiers](../../images/dev-center/title-storage/global-storage-management.gif)

#### <a name="file-types"></a>Types de fichier

* Binaire - le type binaire doit être utilisé pour les images, des sons et des données personnalisées. Ces données doivent être HTTP convivial.
* Config - fichiers de configuration contiennent les informations concernant votre jeu et peuvent avoir une requête dynamique basés sur une entrée de valeurs de retour.
* JSON - fichiers .json. Ces fichiers contiennent des informations sur un aspect de votre jeu, similaire à un fichier de configuration.

## <a name="further-reading"></a>Informations supplémentaires

Pour en savoir plus sur la plateforme de stockage Xbox Live, consultez le [documentation sur le stockage de titre](../../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md) qui donnera plus d’informations sur universel, approuvé, stockage Global et types de fichiers de stockage et le [stockage connecté documentation](../../storage-platform/connected-storage/connected-storage-overview.md), ce qui vous apprendra plus sur l’enregistrement de progression de jeu dans le cloud afin que vous puissiez continuer votre jeu entre les appareils.