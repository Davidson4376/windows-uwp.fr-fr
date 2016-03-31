---
ms.assetid: A7E0DA1E-535A-459E-9A35-68A4150EE9F5
description: Cette rubrique explique comment ajouter la gestion des droits numériques (DRM) en fonction du matériel par PlayReady à votre application pour plateforme Windows universelle (UWP).
title: Gestion des droits numériques en fonction du matériel
---

# Gestion des droits numériques en fonction du matériel

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Cette rubrique explique comment ajouter la gestion des droits numériques (DRM) en fonction du matériel par PlayReady à votre application pour plateforme Windows universelle (UWP).

**Remarque** La gestion des droits numériques en fonction du matériel est uniquement prise en charge sur certains matériels dotés d’une version Windows 10 du microprogramme de ce matériel. Pour plus d’informations sur les garanties fournies, reportez-vous aux [règles de conformité et de robustesse PlayReady](http://www.microsoft.com/playready/licensing/compliance/).

Les fournisseurs de contenu se tournent de plus en plus vers des protections matérielles pour autoriser la lecture de contenu à valeur élevée complet dans les applications. La prise en charge robuste d’une implémentation matérielle de la base de chiffrement a été ajoutée à PlayReady pour répondre à ce besoin. Cette prise en charge permet la lecture sécurisée de contenu en haute définition (1080p) et ultra haute définition (UHD) sur plusieurs plateformes d’appareils. Le matériel de clé (y compris les clés privées, les clés de contenu et tout autre matériel de clé utilisé pour dériver ou déverrouiller ces clés) et les échantillons vidéo compressés et non compressés déchiffrés sont protégés en tirant parti de la sécurité matérielle.

## Implémentation TEE Windows

Cette rubrique offre une présentation succincte de la façon dont Windows 10 implémente l’environnement d’exécution de confiance (TEE).

Les détails de l’implémentation TEE Windows n’entrent pas dans le cadre de ce document. Cependant, une brève description de la différence entre le port TEE du kit de portage standard et le port Windows sera utile. Windows implémente la couche proxy OEM et transfère les appels de fonctions PRITEE en série à un pilote en mode utilisateur dans le sous-système Windows Media Foundation. Ceux-ci seront finalement acheminés vers le pilote TrEE (Trusted Execution Environment) Windows ou le pilote graphique OEM. Les détails de ces deux approches n’entrent pas dans le cadre de ce document. Le diagramme suivant illustre l’interaction générale des composants pour le port Windows. Si vous souhaitez développer une implémentation TEE Windows PlayReady, vous pouvez contacter <WMLA@Microsoft.com>.

![diagramme des composants tee Windows](images/windowsteecomponentdiagram720.jpg)

## Considérations pour l’utilisation de la gestion des droits numériques en fonction du matériel

Cette rubrique fournit une courte liste des éléments à prendre en compte lors du développement d’applications conçues pour utiliser la gestion des droits numériques en fonction du matériel.

-   Le processus de média protégé (PMP) n’est pas pris en charge.
-   Prise en charge du niveau de protection de sortie (norme OPL) 270 (pas de conversion descendante). Microsoft recommande que le contenu haute définition pour la gestion des droits numériques en fonction du matériel présente une norme OPL supérieure à 270 (bien que cela ne soit pas obligatoire). Avec une norme OPL supérieure à 270, la protection HDCP est requise. En outre, Microsoft vous recommande de définir la protection HDCP de type 1 (version 2.2 ou ultérieure).
-   À la différence de la gestion des droits numériques en fonction du logiciel, les protections de sortie sont appliquées sur tous les moniteurs en fonction du moniteur le moins puissant. Par exemple, si l’utilisateur a deux moniteurs connectés dont un seul prend en charge la protection HDCP, la lecture échouera si la licence requiert la protection HDCP, même si le contenu est uniquement affiché sur l’écran qui prend en charge la protection HDCP. Avec la gestion des droits numériques en fonction du matériel, le contenu serait lu tant qu’il est affiché uniquement sur le moniteur qui prend en charge la protection HDCP.
-   L’utilisation par le client et la sécurisation de la gestion des droits numériques en fonction du matériel n’est garantie qu’à condition que les conditions suivantes soient remplies par les clés et les licences de contenu :
    -   L’audio doit être en clair ou chiffré avec une clé de contenu différente de celle de la vidéo. Microsoft recommande que l’audio soit en clair pour améliorer les performances de lecture.
    -   La licence utilisée pour la clé de contenu vidéo doit présenter un niveau de sécurité de 3 000.
-   Les processeurs graphiques (GPU) multiples ne sont pas pris en charge pour les licences persistantes.

Pour gérer les licences persistantes sur des ordinateurs dotés de plusieurs GPU, considérez le scénario suivant :

1.  Un client achète un nouvel ordinateur équipé d’une carte graphique intégrée.
2.  Le client utilise une application qui acquiert les licences persistantes lors de l’utilisation de la gestion des droits numériques en fonction du matériel.
3.  La licence persistante est maintenant liée aux clés matérielles de la carte graphique.
4.  Le client installe ensuite une nouvelle carte graphique.
5.  Toutes les licences du magasin de données hachées (HDS) sont liées à la carte vidéo intégrée, mais le client souhaite maintenant lire du contenu protégé à l’aide de la carte graphique qu’il vient d’installer.

Pour empêcher que la lecture échoue du fait que les licences ne peuvent pas être déchiffrées par le matériel, PlayReady utilise un magasin HDS distinct pour chaque carte graphique rencontrée. PlayReady procédera ainsi à une tentative d’acquisition de licence pour un élément de contenu pour lequel PlayReady aurait normalement déjà une licence (autrement dit, en cas de gestion des droits numériques en fonction du logiciel ou dans toute situation n’impliquant pas de changement de matériel, PlayReady n’aurait pas besoin d’acquérir à nouveau une licence). Par conséquent, si l’application acquiert une licence persistante lors de l’utilisation de la gestion des droits numériques en fonction du matériel, votre application doit pouvoir gérer le cas où cette licence est effectivement « perdue » si l’utilisateur final installe (ou désinstalle) une carte graphique. Comme cela n’est pas un scénario courant, vous pouvez décider de gérer les appels au support lorsque le contenu n’est plus lu après un changement de matériel plutôt que de déterminer comment traiter un changement de matériel dans le code client/serveur.

## Contourner la gestion des droits numériques en fonction du matériel

Cette section explique comment contourner la gestion des droits numériques en fonction du matériel si le contenu à lire ne la prend pas en charge.

Par défaut, la gestion des droits numériques en fonction du matériel est utilisée si le système la prend en charge. Cependant, certains contenus ne sont pas pris en charge par la gestion des droits numériques en fonction du matériel, par exemple le contenu Cocktail, tout contenu qui utilise un codec vidéo autre que H.264 et HEVC, ou encore le contenu HEVC, car certains types de gestion des droits numériques en fonction du matériel prennent en charge le codec HEVC et d’autres non. Par conséquent, si vous voulez lire un élément de contenu et que la gestion des droits numériques en fonction du matériel ne le prend pas en charge sur le système en question, vous voudrez peut-être désactiver la gestion des droits numériques en fonction du matériel.

L’exemple suivant explique comment désactiver la gestion des droits numériques en fonction du matériel. Vous devez uniquement effectuer cette action avant de procéder à la modification. Assurez-vous également que vous n’avez aucun objet PlayReady en mémoire. Sinon, le comportement n’est pas défini.

``` syntax
var applicationData = Windows.Storage.ApplicationData.current;
var localSettings = applicationData.localSettings.createContainer(“PlayReady”, Windows.Storage.ApplicationDataCreateDisposition.always);
localSettings.values[“SoftwareOverride”] = 1;
```

Pour revenir à la gestion des droits numériques en fonction du matériel, définissez la valeur **SoftwareOverride** sur **0**.

Pour chaque lecture multimédia, vous devez définir **MediaProtectionManager** sur :

``` syntax
mediaProtectionManager.properties[“Windows.Media.Protection.UseSoftwareProtectionLayer”] = true;
```

Pour savoir si vous êtes en gestion des droits numériques en fonction du matériel ou en fonction du logiciel, regardez sous C:\\Users\\&lt;username&gt;\\AppData\\Local\\Packages\\&lt;application name&gt;\\LocalState\\PlayReady\\\*

-   Si vous voyez un fichier mspr.hds, cela signifie que vous êtes en gestion des droits numériques en fonction du logiciel.
-   Si vous voyez un autre fichier *.hds, cela signifie que vous êtes en gestion des droits numériques en fonction du matériel.
-   Vous pouvez supprimer l’intégralité du dossier PlayReady et relancer votre test.

## Détecter le type de gestion des droits numériques en fonction du matériel

Cette section explique comment détecter quel type de gestion des droits numériques en fonction du matériel est pris en charge sur le système.

Vous pouvez utiliser la méthode [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441) pour déterminer si le système prend en charge une fonctionnalité spécifique de gestion des droits numériques (DRM) en fonction du matériel. Par exemple :

``` syntax
boolean PlayReadyStatics->CheckSupportedHardware(PlayReadyHardwareDRMFeatures enum);
```

L’énumération [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) contient la liste valide des valeurs de fonctionnalité de gestion des droits numériques en fonction du matériel pouvant être interrogées. Pour déterminer si la gestion des droits numériques en fonction du matériel est prise en charge, utilisez le membre **HardwareDRM** dans la requête. Pour déterminer si le matériel prend en charge le codec HEVC (High Efficiency Video Coding)/H.265, utilisez le membre **HEVC** dans la requête.

Vous pouvez également utiliser la propriété [**PlayReadyStatics.PlayReadyCertificateSecurityLevel**](https://msdn.microsoft.com/library/windows/apps/windows.media.protection.playready.playreadystatics.playreadycertificatesecuritylevel.aspx) pour obtenir le niveau de sécurité du certificat client afin de déterminer si la gestion des droits numériques en fonction du matériel est prise en charge. À moins que le niveau de sécurité renvoyé pour le certificat soit supérieur ou égal à 3 000, le client n’est pas individualisé ou configuré (auquel cas cette propriété renvoie 0) ou la gestion des droits numériques en fonction du matériel n’est pas utilisée (auquel cas cette propriété renvoie une valeur inférieure à 3 000).

<!--HONumber=Mar16_HO1-->
