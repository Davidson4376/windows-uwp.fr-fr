---
author: jnHs
Description: "Vous pouvez publier des applications métier directement à l’attention des entreprises pour une acquisition en volume par le biais du Windows Store pour Entreprises, sans mettre vos applications à disposition sur le Store de façon étendue."
title: "Distribuer des applications métier aux entreprises"
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
translationtype: Human Translation
ms.sourcegitcommit: 9ad7589344d2af986e52ae43acc3e48de6374ae6
ms.openlocfilehash: d7551e0456ce0e59dbbfa92690ddd5ba2ebaf8b4

---

# Distribuer des applications métier aux entreprises


Vous pouvez publier des applications métier directement à l’attention des entreprises pour une acquisition en volume par le biais du Windows Store pour Entreprises, sans mettre vos applications à disposition sur le Windows Store de façon étendue.

> **Important** Actuellement, seules les applications gratuites peuvent être distribuées exclusivement aux entreprises par le biais du Windows Store pour Entreprises. Si vous soumettez une application payante en tant qu’application métier, elle ne sera pas disponible pour l’entreprise à ce stade. 

## Configuration de l’association d’entreprise


Lorsque vous envisagez de publier des applications métier exclusivement à l’attention d’une entreprise, la première étape consiste à établir l’association entre votre compte et le magasin privé de l’entreprise.

> **Important** Ce processus d’association doit être initié par l’entreprise et doit être envoyé à l’adresse de messagerie figurant dans les **Coordonnées** de votre compte. Pour plus d’informations, voir [Utilisation des applications métier](http://go.microsoft.com/fwlink/p/?LinkId=698846).

Lorsqu'une entreprise vous invite à publier des applications destinées à son utilisation exclusive, vous recevez un e-mail contenant un lien pour confirmer l'association. Vous pouvez également confirmer ces associations dans la section **Associations d'entreprise**, sous **Paramètres de compte**.

Pour confirmer l'association, cliquez sur **Accepter**. Votre compte pourra désormais publier des applications destinées à une utilisation exclusive par l'entreprise.

## Soumission d'une application métier


Lorsque vous êtes prêt à publier une application destinée à une utilisation exclusive par une entreprise, vous suivez un processus similaire au processus de soumission d'application. L'application est soumise au même processus de certification et doit être conforme à l'ensemble des [stratégies du Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944). Seuls deux aspects du processus sont différents.

### Distribution et visibilité

Une fois que vous avez configuré une association d’entreprise, dès lors que vous soumettez une application, une liste déroulante s’affiche dans la section **Distribution et visibilité** de la page **Tarification et disponibilité** relative à la soumission. La sélection par défaut est **Vente au détail**. Pour mettre l’application à la disposition exclusive d’une entreprise, vous devez choisir **Distribution d’applications métier**.

Une fois l’option **Distribution d’applications métier** sélectionnée, les options **Distribution et visibilité** habituelles sont remplacées par une liste des entreprises pour lesquelles vous pouvez publier des applications exclusives.

Sélectionnez les entreprises qui doivent pouvoir accéder à l'application. Elle ne sera accessible à personne d’autre.

> **Remarque** Vous devez sélectionner au moins une entreprise pour publier une application en tant qu’application métier.

### Gestion des licences organisationnelles

Par défaut, la case **Proposer mon application aux organisations via le service de gestion de licences en volume (en ligne) du Store** est cochée lorsque vous soumettez une application. Lorsque vous soumettez des applications métier, cette case doit être cochée, de sorte que l’entreprise puisse acquérir votre application en volume. Personne ne pourra y accéder, hormis les entreprises que vous avez sélectionnées dans la section **Distribution et visibilité**.

Si vous souhaitez mettre l’application à disposition de l’entreprise à l’aide de licences en mode hors connexion, vous pouvez également cocher la case **Permettre aux entreprises d’acheter des licences en mode hors connexion**.

Pour plus d'informations, voir [Options de gestion des licences organisationnelles](organizational-licensing.md).

### Déploiement d'applications métier dans les entreprises

Lorsque vous cliquez sur **Envoyer au Store**, le processus de certification de l'application s'exécute. À l'issue de ce processus, un administrateur de l'entreprise doit l'ajouter à son magasin privé dans le portail Windows Store pour Entreprises. L'entreprise peut alors déployer l'application à l'attention de ses utilisateurs.

> **Remarque** Pour obtenir votre application métier, l’organisation doit se trouver dans un [marché pris en charge](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets), et vous ne devez pas avoir exclu ce marché lors de la soumission de votre application. 

Pour plus d'informations, voir [Utilisation des applications métier](http://go.microsoft.com/fwlink/p/?LinkId=698846) et [Distribuer des applications à l'aide de votre magasin privé](http://go.microsoft.com/fwlink/p/?LinkId=698847).

### Mise à jour des applications métier

Pour publier les mises à jour d'une application que vous avez déjà publiée en tant qu'application métier, il vous suffit de créer une soumission. Vous pouvez transférer de nouveaux packages ou apporter des modifications, puis cliquer sur **Envoyer au Store** pour mettre à disposition la version mise à jour. Veillez à ce que les sélections d'entreprises sous **Distribution et visibilité** restent les mêmes (sauf si vous souhaitez les modifier, par exemple pour sélectionner une autre entreprise pouvant acquérir l'application ou supprimer l'une des entreprises auxquelles vous l'avez déjà distribuée).

Si vous souhaitez ne plus offrir une application que vous avez déjà publiée en tant qu’application métier et que vous souhaitez empêcher toute nouvelle acquisition, vous devez créer une soumission. En premier lieu, vous devez modifier votre sélection sous **Distribution et visibilité** et choisir **Vente au détail** au lieu de **Distribution d’applications métier**. Ensuite, sous **Distribution et visibilité**, choisissez l'option **Masquer cette application et empêcher l'acquisition**. Une fois le processus de certification appliqué à la soumission, l’application n’est plus disponible pour de nouvelles acquisitions (les personnes qui en disposent déjà pourront cependant continuer à l’utiliser).

### Distribution d'applications métier par chargement indépendant

En mettant votre application à disposition par le biais du Windows Store pour Entreprises, vous vous assurez qu'elle a été signée par le Store et qu'elle est conforme aux stratégies standard du Store.

Dans certains cas, les entreprises ne souhaitent pas soumettre leurs applications métier par le biais du Centre de développement Windows, et ce, pour différentes raisons (par exemple, pour des raisons de conformité ou lorsque les applications requièrent des fonctionnalités supplémentaires). Les entreprises peuvent alors déployer des applications directement sur les machines par chargement indépendant, sans utiliser le Windows Store pour Entreprises.

Pour plus d'informations, voir [Charger la version test d'applications métier dans Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 







<!--HONumber=Jun16_HO5-->


