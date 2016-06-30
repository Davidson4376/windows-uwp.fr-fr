---
author: mcleanbyron
ms.assetid: 69E05E56-B5F0-4D4C-A1FF-B6EAFF5D0E28
description: "Lors du processus de soumission, vous pouvez configurer le comportement de médiation publicitaire que vous souhaitez. Vous pourrez l’ajuster ultérieurement sans devoir modifier le code ni soumettre de nouveaux packages."
title: "Soumettre votre application et configurer une médiation publicitaire"
translationtype: Human Translation
ms.sourcegitcommit: ec7ce299545de8e5c167e1934fb9a0b4f4370948
ms.openlocfilehash: 13dd6a9c38d85ead29a43f470b7c0f63d025d612

---

# Soumettre votre application et configurer une médiation publicitaire


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Une fois que vous avez développé votre application pour inclure tous les réseaux publicitaires que vous voudrez peut-être utiliser et que vous l’avez testée pour vérifier son bon fonctionnement, vous êtes prêt à la soumettre. Lors du processus de soumission, vous pouvez configurer le comportement de médiation publicitaire que vous souhaitez. Vous pourrez l’ajuster ultérieurement sans devoir modifier le code ni soumettre de nouveaux packages.

## Créer une configuration de base pour un package d’application


Lorsque vous chargez vos packages, le Centre de développement détecte automatiquement que vous utilisez une médiation publicitaire et identifie les réseaux publicitaires utilisés. Sur la page **Packages**, vous verrez le lien **Configurer la médiation publicitaire**. Cliquez sur ce lien pour accéder à la page [Monétiser avec des publicités](https://msdn.microsoft.com/library/windows/apps/mt170658), sur laquelle vous pourrez configurer votre logique de médiation dans la section **Médiation publicitaire Windows**. Pour en savoir plus sur les autres sections de cette page, voir Monétisation à l’aide des publicités.

La première fois que vous configurez les paramètres de médiation publicitaire pour votre application, vous créez une configuration de référence. Vous pouvez ensuite ajouter des configurations propres aux marchés pour tirer parti des atouts des réseaux publicitaires spécifiques sur les différents marchés. Si vous choisissez de ne pas configurer vos paramètres de médiation publicitaire avant de soumettre votre application, le Centre de développement applique automatiquement les [paramètres par défaut](#default-ad-mediation-settings), que vous pourrez [modifier ultérieurement](#optimize-ad-mediation-after-the-app-has-been-published).

Les étapes suivantes décrivent comment créer une configuration de base dans la section **Médiation publicitaire Windows** de la page **Monétiser avec des publicités**.

1.  Sous **Configurer la médiation pour**, vérifiez que le package d’application que vous voulez configurer est sélectionné.
2.  Sous **Cible**, assurez-vous que l’option **De base** est sélectionnée.
3.  Sous **Fréquence d’actualisation**, définissez la durée du cycle de médiation (fréquence à laquelle les nouvelles publicités doivent être affichées). Cette durée doit être comprise entre 30 et 120 secondes.
  > **Remarque** Si vous avez déjà configuré une fréquence d’actualisation dans l’un de vos portails de réseau publicitaire, vérifiez que vous définissez la même fréquence d’actualisation ici.

4.  Ensuite, la liste **Médiation publicitaire Windows** répertorie tous les réseaux publicitaires utilisés par votre application et propose deux façons de spécifier la fréquence à laquelle votre application doit utiliser chaque réseau. Choisissez l’une de ces options dans la liste déroulante **Type de médiation** :

    -   **Classer par poids**. Choisissez cette option pour appliquer des valeurs de pourcentage à chaque réseau publicitaire pour indiquer à quelle fréquence l’application doit les utiliser. Le total des pourcentages que vous définissez pour tous les réseaux publicitaires doit correspondre exactement à 100 %. Pour en savoir plus, voir [Classer les réseaux publicitaires par poids](#order-ad-networks-by-weight).
    -   **Classer par classement**. Choisissez cette option pour appliquer un numéro de classement à chaque réseau publicitaire, de 1 à *n*, qui spécifie la fréquence à laquelle votre application doit utiliser chaque réseau. Pour en savoir plus, voir [Classer les réseaux publicitaires par classement](#order-ad-networkds-by-rank).

    Le Centre de développement applique automatiquement les [paramètres par défaut](#default-ad-mediation-settings), qui spécifient la fréquence à laquelle votre application doit utiliser chaque réseau publicitaire.

5.  Dans la liste des réseaux publicitaires utilisés par votre application, cliquez sur la flèche déroulante vers le bas de chaque fournisseur de réseau publicitaire afin d’afficher les paramètres obligatoires pour chaque réseau, puis entrez les paramètres obligatoires. Pour obtenir la liste des paramètres, voir [Sélectionner et gérer vos réseaux publicitaires](select-and-manage-your-ad-networks.md).

    Dans la liste développée des paramètres de chaque réseau, vous pouvez éventuellement utiliser le champ **Délai d’attente** pour spécifier le nombre de secondes (de 2 à 120) définissant le temps d’attente de la médiation publicitaire après qu’elle a demandé une publicité au réseau publicitaire, avant qu’elle n’abandonne cette demande et n’en adresse une autre à un autre réseau. Si vous avez déjà [spécifié cette valeur dans votre code](add-and-use-the-ad-mediator-control.md#set-timeouts), la valeur spécifiée dans le code remplace la valeur définie ici.

    Si vous utilisez Microsoft Advertising, notez les éléments suivants :

    -   Les paramètres de **Microsoft Advertising** sont renseignés pour vous en fonction du contenu de votre package d’application. Vous pouvez éventuellement spécifier vos propres valeurs d’**ID de l’application** et d’**ID de l’unité de publicité** pour **Microsoft Advertising**.
    -   Outre **Microsoft Advertising**, il existe aussi une ligne pour **les publicités maison Microsoft Advertising**. Les publicités maison permettent de créer des publicités gratuites qui promeuvent l’une de vos applications dans vos autres applications. Bien que les publicités maison soient gratuites, votre allocation provient du même pool que les autres réseaux publicitaires. Par conséquent, si vous allouez des demandes de publicité aux **Publicités maison Microsoft Advertising**, vous choisissez d’appliquer ce pourcentage de votre dotation publicitaire totale aux publicités maison. Si vous allouez des demandes de publicité pour les publicités maison, vous devez également créer une campagne de publicité maison pour au moins une autre application enregistrée dans votre compte de développeur. Pour plus d’informations, voir [À propos des publicités maison](https://msdn.microsoft.com/library/windows/apps/mt148566).

6.  Cliquez sur **Enregistrer** pour enregistrer la configuration de base.

### Classer des réseaux publicitaires par poids

Choisissez cette option dans la liste déroulante **Type de médiation** pour spécifier les valeurs de pourcentage indiquant la fréquence à laquelle votre application doit utiliser chaque réseau publicitaire. Le total des pourcentages que vous définissez pour tous les réseaux publicitaires doit correspondre exactement à 100 %.

Pour répartir automatiquement les demandes de manière égale entre tous vos réseaux, cliquez sur **Distribuer équitablement les demandes publicitaires sur tous vos réseaux publicitaires actifs**. Pour répartir les demandes de manière inégale, utilisez les curseurs pour indiquer la fréquence à laquelle votre application doit utiliser chaque réseau publicitaire. Le total des pourcentages que vous définissez pour tous les réseaux publicitaires doit correspondre exactement à 100 %. Lorsque vous faites glisser les curseurs, vous disposez de plusieurs options :

-   Si vous faites glisser le curseur vers un chiffre, celui-ci indique le pourcentage de temps pendant lequel ce réseau publicitaire est appelé en tant que premier choix de l’application dans un cycle de médiation.
-   Si vous faites glisser le curseur vers **Réserve**, cela indique que le réseau publicitaire doit être appelé uniquement si aucun des réseaux publicitaires dotés d’un pourcentage désigné ne peut fournir de publicité. Cela revient à définir le pourcentage sur 0 %.
-   Si vous faites glisser le curseur vers **Inactif**, cela indique que ce réseau publicitaire ne sera jamais appelé. Les assemblys du réseau publicitaire restent dans le package, mais le médiateur ne tente pas de les appeler. Vous pouvez définir cette option dans des configurations propres aux marchés pour exclure les réseaux publicitaires qui sont connus pour leurs faibles résultats ou qui ne prennent pas en charge un certain marché.
-   Lorsque vous ajustez le pourcentage d’un réseau publicitaire, n’importe quel autre contrôle de curseur dans lequel vous avez sélectionné une valeur autre que **Réserve** s’ajuste automatiquement afin que la distribution totale atteigne 100 %. Si vous coché la case **Verrouillage** pour un réseau publicitaire, ce dernier conserve sa valeur actuelle. Vous pouvez ajuster les valeurs des autres réseaux publicitaires sans affecter la valeur du réseau publicitaire verrouillé

### Classer des réseaux publicitaires par classement

Choisissez cette option dans la liste déroulante **Type de médiation** pour appliquer un numéro de classement à chaque réseau publicitaire, de 1 à *n*, qui spécifie la fréquence à laquelle votre application doit utiliser chaque réseau publicitaire.

En décochant la case **Actif** d’un réseau publicitaire, vous indiquez que ce réseau ne sera jamais appelé. Les assemblys du réseau publicitaire restent dans le package, mais le médiateur ne tente pas de les appeler. Vous pouvez définir cette option dans des configurations propres aux marchés pour exclure les réseaux publicitaires qui sont connus pour leurs faibles résultats ou qui ne prennent pas en charge un certain marché.

### Paramètres de médiation publicitaire par défaut

Par défaut, le Centre de développement applique les paramètres de médiation publicitaire suivants à votre application sur la page **Monétisation à l’aide des publicités**. Ces mêmes valeurs par défaut sont également appliquées si vous choisissez de ne pas configurer la médiation publicitaire avant de soumettre votre application.

-   Si votre application utilise Microsoft Advertising, l’option **Microsoft Advertising** est définie sur 100 (si vous choisissez **Classer par poids**) ou 1 (si vous choisissez **Classer par classement**), et tous les autres réseaux publicitaires sont définis sur Inactif.
-   Si votre application n’utilise pas Microsoft Advertising, tous les réseaux publicitaires sont définis sur Inactif.

## Créer une nouvelle configuration cible


Après avoir créé une configuration de base pour votre application, vous pouvez créer des configurations supplémentaires à utiliser sur des marchés spécifiques. La configuration de base fournit à ces nouvelles configurations leurs paramètres d’origine, mais vous pouvez ajuster les détails pour les marchés spécifiques que vous prenez en charge pour votre application.

Pour créer une configuration cible, procédez comme suit :

1.  Sous **Cible**, sélectionnez le marché pour lequel vous voulez créer la configuration.
2.  Mettez à jour le taux d’actualisation et les informations de distribution de demande publicitaire selon vos besoins.
3.  Cliquez sur **Enregistrer** pour enregistrer la nouvelle configuration.

## Optimiser la médiation publicitaire après la publication de l’application


Si vous le souhaitez, vous pouvez ajuster votre médiation publicitaire pour une application spécifique à tout moment sans avoir à soumettre de nouveau l’application. Cela est utile si vous avez déjà ajouté des réseaux publicitaires dans votre application pour laquelle vous n’aviez pas configuré de comptes ou si vous estimez qu’un réseau publicitaire n’est pas en mesure de remplir les publicités de façon fiable sur des marchés spécifiques. Vous pouvez apporter des modifications à votre configuration de référence ainsi qu’aux configurations propres aux marchés. Vous pouvez également ajouter ou supprimer des configurations propres aux marchés si vous le souhaitez.

Pour rétablir les paramètres de médiation publicitaire pour votre application, effectuez l’une des opérations suivantes :

-   Dans la page de vue d’ensemble de votre compte sur le tableau de bord, cliquez sur votre application sous la section **Médiation publicitaire**.
-   Dans la page de vue d’ensemble de votre compte sur le tableau de bord, développez **Monétisation**, puis cliquez sur **Monétisation à l’aide des publicités**.

## Paramètres de médiation publicitaire et mises à jour des applications


Lorsque vous envoyez une mise à jour de l’application, les informations de configuration de la médiation publicitaire existantes pour votre application sont automatiquement appliquées au package mis à jour si les conditions suivantes sont remplies :

-   Le nombre de contrôles **AdMediatorControl** est le même que le nombre de contrôles dans le package d’application précédent, et tous les contrôles ont les mêmes ID que le package d’application précédent.
-   La liste des fournisseurs de réseaux publicitaires est la même que celle du package d’application précédent.

Si l’une de ces conditions n’est pas respectée, vous devez recréer la configuration de base et les configurations cibles propres au marché applicables à votre application.

> **Remarque** L’ID d’un **AdMediatorControl** est généré lorsque vous faites glisser le contrôle vers une aire de conception dans votre application. Cet ID ne change pas, sauf si vous supprimez le contrôle et que vous le remplacez en faisant glisser un nouveau contrôle vers la même aire de conception.

 

## Examiner les rapports de médiation publicitaire du tableau de bord du Centre de développement unifié


Pour consulter les rapports de médiation publicitaire, accédez à la section **Analyse** du Centre de développement, puis cliquez sur **Médiation publicitaire**. Pour plus d’informations sur ce rapport, voir [Rapport de médiation publicitaire](https://msdn.microsoft.com/library/windows/apps/mt148521).

## Exemples de scénario


Vous pouvez configurer vos réseaux publicitaires de différentes manières pour prendre en charge des scénarios et des objectifs variés. Les exemples ci-dessous montrent comment vous pouvez configurer votre médiation publicitaire à l’aide de **Classer par poids** lorsque vous avez inclus 3 réseaux publicitaires. Nous allons utiliser Microsoft Advertising, Inneractive et AdDuplex à titre d’exemples de réseau.

### Exemple 1

Vous voulez déterminer le taux de remplissage et le coût eCPM de tous les réseaux, avant de commencer l’optimisation.

Pour commencer, définissez chaque réseau pour utiliser une distribution égale du trafic de médiation. Vous pouvez consulter ultérieurement les rapports de chaque réseau publicitaire pour déterminer les réseaux publicitaires les plus performants de chaque marché.

Après quelques jours ou quelques semaines, vous vérifierez le taux de remplissage et le coût eCPM dans chaque portail de réseau publicitaire. Cela vous aidera à déterminer les meilleurs réseaux publicitaires de chaque marché. Vous pouvez ensuite effectuer des ajustements pour des marchés spécifiques (ou généraux) sans avoir à soumettre de nouveaux packages.

### Exemple 2

Vous voulez utiliser Microsoft Advertising en premier lieu autant que possible. Si Microsoft Advertising ne peut pas fournir de publicité, vous serez satisfait d’utiliser l’un de vos autres réseaux publicitaires en tant que réseau de réserve, sans aucune préférence pour un réseau particulier.

| Réseau publicitaire            | Configuration |
|-----------------------|---------------|
| Microsoft             | 100%          |
| Inneractive           | Sauvegarde        |
| AdDuplex              | Sauvegarde        |

### Exemple 3

Vous souhaitez généralement utiliser Microsoft Advertising en premier. Si Microsoft Advertising ne peut pas fournir de publicité, vous voulez vous assurer qu’Inneractive sera appelé avant AdDuplex.

| Réseau publicitaire            | Configuration |
|-----------------------|---------------|
| Microsoft             | 90%           |
| Inneractive           | 10 %           |
| AdDuplex              | Sauvegarde        |

### Example 4

Vous souhaitez utiliser Microsoft Advertising et Inneractive de façon équitable pour qu’il existe une plus grande variété de publicités dans votre application que si vous appeliez toujours un même réseau en premier. Si aucun réseau publicitaire n’est disponible, vous utiliserez AdDuplex en tant que sauvegarde.

| Réseau publicitaire            | Configuration |
|-----------------------|---------------|
| Microsoft             | 50%           |
| Inneractive           | 50%           |
| AdDuplex              | Sauvegarde        |


## Rubriques connexes

* [Sélectionner et gérer vos réseaux publicitaires](select-and-manage-your-ad-networks.md)
* [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md)
* [Tester l’implémentation de votre médiation publicitaire](test-your-ad-mediation-implementation.md)
* [Résoudre les problèmes liés à la médiation publicitaire](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


