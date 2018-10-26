---
author: jnHs
Description: The App properties page of the app submission process lets you define your app's category and indicate hardware preferences or other declarations.
title: Entrer les propriétés d’une application
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
keywords: windows10, uwp, paramètres de jeu, mode d’affichage, configuration requise, configuration matérielle requise, matériel minimum, matériel recommandé, politique de confidentialité, coordonnées du support technique, site web de l’application, support
ms.localizationpriority: medium
ms.openlocfilehash: 3b7fffe4c687009f71c9908ca9391278a172209a
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5567310"
---
# <a name="enter-app-properties"></a>Entrer les propriétés d’une application

La page **Propriétés** du [processus de soumission d’applications](app-submissions.md) vous permet de définir la catégorie de votre application, et de spécifier d’autres informations et déclarations. Veillez à fournir sur cette page des informations complètes et précises sur votre application.


## <a name="category-and-subcategory"></a>Catégorie et sous-catégorie

Vous devez indiquer la catégorie (et la sous-catégorie/le genre s'il y a lieu) dans laquelle le Store doit classer votre application. Vous devez définir une catégorie pour être en mesure de soumettre votre application.

Pour plus d’informations, voir l’article [Tableau des catégories et sous-catégories](category-and-subcategory-table.md).


## <a name="support-info"></a>Coordonnées du support technique

Cette section vous permet de fournir des informations pour aider les clients à mieux comprendre votre application et à obtenir un support.

### <a name="privacy-policy-url"></a>URL de la politique de confidentialité

Il vous incombe de veiller à ce que votre application respecte les lois et règles en vigueur en matière de confidentialité, et de proposer ici une URL valide de la politique de confidentialité s’il y a lieu.

Dans cette section, vous devez indiquer si votre application accède à, collecte ou transmet des [informations personnelles](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information). Si vous répondez **Oui**, une URL de politique de confidentialité est requise. Dans le cas contraire, elle est facultative (cependant, si nous déterminons que votre application nécessite une politique de confidentialité et que vous n’en avez indiqué aucune, votre soumission risque de ne pas obtenir de certification).

> [!NOTE]
> Si nous détectons que vos packages déclarent des [fonctionnalités](../packaging/app-capability-declarations.md) qui pourraient permettre l’accès à, la transmission ou la collecte d’informations personnelles, cette question sera marquée comme **Oui** et vous devrez entrer une URL de politique de confidentialité.

Pour déterminer plus facilement si votre application requiert une politique de confidentialité, examinez le [Contrat du développeur d’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) et les [politiques du MicrosoftStore](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information). 

> [!NOTE]
> Microsoft ne fournit aucune politique de confidentialité par défaut pour votre application. De même, votre application n’est couverte par aucune politique de confidentialité Microsoft. 


### <a name="website"></a>Site web

Entrez l’URL de la page web de votre application. Cette URL doit pointer vers une page de votre propre site web, et non vers la description web de votre application dans le Store. Ce champ est facultatif, mais recommandé.

### <a name="support-contact-info"></a>Coordonnées du support technique

Entrez l’URL de la page web où vos clients peuvent obtenir un support pour votre application (ou une adresse e-mail pour contacter le support technique). Nous vous recommandons d’inclure ces informations pour toutes les soumissions, afin que vos clients sachent comment obtenir un support s’ils en ont besoin. Notez que Microsoft ne fournit pas à vos clients de support technique pour votre application.

> [!IMPORTANT]
> Le champ **Coordonnées du support technique** est obligatoire si votre application ou votre jeu est disponible sur Xbox. Sinon, il est facultatif (mais recommandé).


## <a name="game-settings"></a>Paramètres de jeux

Cette section s’affiche uniquement si vous avez sélectionné **Jeux** comme catégorie de votre produit. Ici, vous pouvez spécifier les fonctionnalités que votre jeu prend en charge. Toutes les informations que vous fournissez dans cette section figureront dans la description du produit dans le Windows Store.

Si votre jeu prend en charge des options multijoueurs, veillez à indiquer le nombre minimum et maximum de joueurs pour une session. Vous ne pouvez pas entrer un nombre minimum ou maximum de joueurs supérieur à 1000.

**Mode multijoueur multiplateforme** signifie que le jeu prend en charge des sessions multijoueurs entre joueurs sur PC Windows10 et Xbox.


## <a name="display-mode"></a>Mode d’affichage

Cette section vous permet d’indiquer si votre produit est conçu pour s’exécuter dans un affichage immersif (pas 2D) pour [Windows Mixed Reality](https://developer.microsoft.com/windows/mixed-reality) sur les PC et/ou les appareils HoloLens. Si tel est le cas, vous devrez également:
- Sélectionner **Matériel minimum** ou **Matériel recommandé** pour le **casque immersif Windows Mixed Reality** dans la section [Configuration requise](#system-requirements) qui s’affiche plus bas dans la page **Propriétés**.
- Spécifiez la **Configuration de délimitation** (si PC est sélectionné) afin que les utilisateurs sachent s’ils doivent l'utiliser dans une position assise ou debout uniquement, ou s’ils sont autorisés à (ou obligés de) se déplacer lors de son utilisation. 

Si vous avez sélectionné **Jeux** pour la catégorie de votre produit, vous verrez des options supplémentaires dans la sélection **Mode d’affichage** qui vous permettent d’indiquer si votre produit prend en charge la sortie vidéo en résolution 4K, la sortie vidéo avec plage dynamique étendue (HDR) ou les affichages avec fréquence de rafraîchissement variable.

Si votre produit ne prend en charge aucune de ces options de mode d’affichage, laissez toutes les cases désactivées.


## <a name="product-declarations"></a>Déclarations de produit

Les cases à cocher de cette section vous permettent d’indiquer les déclarations qui s’appliquent à votre application. Votre sélection affecte le mode d'affichage de votre application, le public auquel elle est proposée ou la façon dont les clients peuvent l'utiliser.

Pour plus d’informations, voir l’article [Déclarations de produit](app-declarations.md).

## <a name="system-requirements"></a>Configuration requise

Cette section vous permet d’indiquer si certaines fonctionnalités matérielles sont requises ou recommandées pour exécuter votre application et interagir avec cette dernière. Lorsque vous souhaitez spécifier **Matériel minimum** et/ou **Matériel recommandé** pour un composant matériel, activez la case à cocher (ou indiquez l’option appropriée).

Si vous effectuez des sélections pour **Matériel recommandé**, ces composants sont affichés dans la description du produit sur le Windows Store en tant que matériel recommandé pour les clients utilisant Windows10, version1607 ou ultérieure. Les clients utilisant des versions antérieures du système d’exploitation ne voient pas ces informations.

Si vous effectuez des sélections pour **Matériel minimum**, ces composants sont affichés dans la description du produit sur le Windows Store en tant que matériel minimum requis pour les clients utilisant Windows10, version1607 ou ultérieure. Les clients utilisant des versions antérieures du système d’exploitation ne voient pas ces informations. Le Windows Store peut également afficher un avertissement pour les clients qui consultent la description de votre application sur un appareil ne disposant pas du matériel requis. Les clients peuvent quand même télécharger votre application sur des appareils non équipés du matériel nécessaire, mais ils ne sont pas en mesure d’évaluer ou de commenter votre application sur ces appareils. 

Le comportement des clients varie selon les configurations requises spécifiques et la version de Windows du client:

- **Pour les clients utilisant Windows10, version1607 ou ultérieure:**
     - La configuration minimale requise et recommandée est affichée dans la description du Windows Store.
     - Le Windows Store vérifie la configuration minimale requise et affiche un avertissement si l’appareil du client ne satisfait pas ces exigences.
- **Pour les clients utilisant des versions antérieures de Windows10:**
     - Pour la plupart des clients, la configuration matérielle minimale requise et recommandée s’affiche dans la description au sein du Windows Store (toutefois les clients utilisant une version antérieure du client Windows Store voient seulement la configuration matérielle minimale requise).
     - Le Windows Store tente de vérifier les composants que vous spécifiez dans **Matériel minimum**, à l’exception de **Mémoire**, **DirectX**, **Mémoire vidéo**, **Carte graphique** et **Processeur**. Aucun de ces composants n’est vérifié, et les clients ne voient aucun avertissement sur les appareils qui ne satisfont pas ces exigences. 
- **Pour les clients utilisant Windows8.x et des versions antérieures ou Windows Phone8.x et des versions antérieures:**
     - Si vous activez la case à cocher **Matériel minimum** pour **Écran tactile**, cette configuration requise s’affiche dans la description de votre application sur le Windows Store, et les clients dont les appareils n’ont pas d’écran tactile voient un avertissement s’ils tentent de télécharger l’application. Aucune autre configuration requise n’est vérifiée ou affichée dans votre description au sein du Windows Store.

Nous vous recommandons également d’ajouter des vérifications d’exécution pour le matériel spécifié dans votre application, car il peut arriver que le Windows Store ne puisse pas détecter que l’appareil d’un client ne dispose pas des fonctionnalités sélectionnées. Dans ce cas, le client peut toujours télécharger votre application, même si un avertissement s’affiche. Si vous souhaitez empêcher complètement le téléchargement de votre application UWP sur un appareil non conforme à la configuration minimale requise concernant la mémoire ou le niveauDirectX, vous pouvez indiquer cette configuration minimale requise dans un [fichierXML StoreManifest](https://docs.microsoft.com/uwp/schemas/storemanifest/storemanifestschema2015/schema-root).

> [!TIP]
> Si votre produit requiert des éléments supplémentaires qui ne sont pas répertoriés dans cette section afin de s’exécuter correctement, tels que des imprimantes3D ou des périphériques USB, vous pouvez également saisir une [configuration système supplémentaire requise](create-app-store-listings.md#additional-system-requirements) lorsque vous créez votre description dans le Store.





