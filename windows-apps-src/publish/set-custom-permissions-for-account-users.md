---
Description: Définir les rôles ou autorisations personnalisées pour les utilisateurs de compte.
title: Définir des rôles ou des autorisations personnalisés pour les utilisateurs de compte
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, rôles d’utilisateur, autorisation d’utilisateur, rôles personnalisés, accès utilisateur, personnaliser les autorisations, rôles standard
ms.localizationpriority: medium
ms.openlocfilehash: dc2786df57ebe1364f045795c52b301ef5feed0f
ms.sourcegitcommit: 94316b8daf60a89a90a661ea17428c3d2734fee8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66814406"
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>Définir des rôles ou des autorisations personnalisés pour les utilisateurs de compte

Lorsque vous [ajouter des utilisateurs à votre compte espace partenaires](add-users-groups-and-azure-ad-applications.md), vous devez spécifier quel accès au sein du compte. Pour effectuer cette opération, vous pouvez attribuer aux utilisateurs des [rôles standard](#roles) qui s’appliquent à la totalité du compte, ou vous pouvez [personnaliser leurs autorisations](#custom) afin de leur fournir le niveau d’accès approprié. Certaines des autorisations personnalisées s’appliquent à l’ensemble du compte, tandis que d’autres peuvent être limitées à un ou plusieurs produits spécifiques (ou accordées à tous les produits si vous préférez ce cas de figure).

> [!NOTE] 
> Vous pouvez appliquer les mêmes rôles et autorisations, que vous ajoutiez un utilisateur, un groupe ou une application Azure AD.

Lorsque vous déterminez le rôle ou les autorisations à appliquer, gardez à l’esprit les points suivants : 
-   Les utilisateurs (y compris les groupes et applications Azure AD) sera en mesure d’accéder à l’intégralité du compte Centre de partenaires avec les autorisations associées à leur rôle attribué (s), à moins que vous [personnaliser les autorisations](#custom) et affecter [ autorisations au niveau du produit](#product-level-permissions) afin qu’il puisse uniquement fonctionner avec des applications spécifiques et/ou des modules complémentaires.
-   Vous pouvez autoriser un utilisateur, un groupe ou une application Azure AD à accéder aux fonctionnalités de différents rôles en sélectionnant plusieurs rôles ou en utilisant les autorisations personnalisées pour accorder l’accès de votre choix.
-   Un utilisateur ayant un certain rôle (ou un ensemble d’autorisations personnalisées) peut également faire partie d’un groupe ayant un rôle différent (ou un ensemble d’autorisations). Dans ce cas, l’utilisateur a accès à toutes les fonctionnalités associées au groupe et au compte individuel.

> [!TIP]
> Cette rubrique est spécifique du programme de développeur d’applications Windows dans [partenaires](https://partner.microsoft.com/dashboard). Pour plus d’informations sur les rôles d’utilisateurs dans le Programme pour développeurs de matériel, voir [Gestion des rôles d’utilisateurs](https://docs.microsoft.com/windows-hardware/drivers/dashboard/managing-user-roles). Pour plus d’informations sur les rôles d’utilisateurs dans le Programme pour applications de bureau Windows, voir [Programme pour applications de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users).


<span id="roles" />

## <a name="assign-roles-to-account-users"></a>Attribuer des rôles aux utilisateurs de compte

Par défaut, un ensemble de rôles standards est présenté pour votre choix lorsque vous ajoutez un utilisateur, un groupe ou une application Azure AD à votre compte espace partenaires. Chaque rôle dispose d’un ensemble d’autorisations spécifique lui permettant d’exécuter certaines fonctions dans le cadre du compte. 

À moins que vous ne choisissiez de définir des [autorisations personnalisées](#custom) en sélectionnant **Personnaliser les autorisations**, chaque utilisateur, groupe ou application Azure AD que vous ajoutez à un compte doit se voir attribuer au moins l’un des rôles standard ci-après. 

> [!NOTE]
> Le **propriétaire** du compte est la personne qui l’a créé en premier avec un compte Microsoft (et non l’un des utilisateurs ajoutés par le biais d’Azure AD). Il est le seul à disposer d’un accès complet au compte et à pouvoir notamment supprimer des applications, créer et modifier l’ensemble des utilisateurs du compte et modifier tous les paramètres financiers et de compte. 


| Rôle                 | Description              |
|----------------------|--------------------------|
| Manager              | Dispose d’un accès complet au compte, mais ne peut pas modifier les paramètres fiscaux et de revenus. Cela inclut la gestion des utilisateurs dans l’espace partenaires, mais notez que la possibilité de créer et supprimer des utilisateurs dans le locataire Azure AD dépend de l’autorisation du compte dans Azure AD. Autrement dit, si un utilisateur se voit attribuer le rôle Manager, mais n’a pas d’autorisations d’administrateur général dans l’organisation Azure AD, ils ne pourrez pas créer de nouveaux utilisateurs ou supprimer des utilisateurs à partir du répertoire (mais ils peuvent modifier le rôle de partenaires d’un utilisateur). <p> Notez que si le compte espace partenaires est associé à plusieurs locataires Azure AD, un gestionnaire ne peut pas voir tous les détails pour un utilisateur (y compris le prénom, nom, e-mail de récupération de mot de passe, et s’ils sont administrateur global Azure AD), sauf si elles sont connecté au même locataire en tant que cet utilisateur avec un compte disposant des autorisations d’administrateur général pour ce client. Cependant, ils peuvent ajouter et supprimer des utilisateurs dans n’importe quel client qui est associé au compte de partenaires. |
| Développeur            | Peut charger des packages, soumettre des applications et modules complémentaires et afficher le [Rapport d’utilisation](usage-report.md) pour obtenir des informations de télémétrie détaillées. Peut accéder aux [inter-périphériques expériences](https://go.microsoft.com/fwlink/?linkid=874042) fonctionnalité. Il ne peut afficher ni les informations financières ni les paramètres de compte.   |
| Contributeur professionnel | Peut afficher des rapports [d’intégrité](health-report.md) et [d’utilisation](usage-report.md). Impossible de créer ou soumettre des produits, de modifier des paramètres de compte ou d’afficher des informations financières.   |
| Contributeur financier  | Peut afficher des [rapports sur les revenus](payout-summary.md), des informations financières et des rapports d’acquisition. Il ne peut apporter aucune modification aux applications, modules complémentaires et paramètres de compte.    |
| Responsable marketing             | Peut [répondre aux avis de clients](respond-to-customer-reviews.md) et afficher des [rapports analytiques](analytics.md) non financiers. Il ne peut apporter aucune modification aux applications, modules complémentaires et paramètres de compte.      |

Le tableau ci-dessous présente certaines fonctionnalités spécifiques disponibles pour chacun de ces rôles (et pour le propriétaire du compte).

|                                                       |    Propriétaire du compte                 |    Manager                       |    Développeur                     |    Contributeur professionnel    |    Contributeur financier    |    Responsable marketing                      |
|-------------------------------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    Rapport d’acquisition (y compris les données quasiment en temps réel) |    Peut afficher                      |    Peut afficher                      |    Aucun accès                     |    Aucun accès               |    Peut afficher               |    Aucun accès                     |
|    Rapport de commentaires/réponses                          |    Peut afficher et envoyer des commentaires    |    Peut afficher et envoyer des commentaires    |    Peut afficher et envoyer des commentaires    |    Aucun accès               |    Aucun accès              |    Peut afficher et envoyer des commentaires    |
|    Rapport d’intégrité (y compris les données quasiment en temps réel)      |    Peut afficher                      |    Peut afficher                      |    Peut afficher                      |    Peut afficher                |    Aucun accès              |    Aucun accès                     |
|    Rapport sur l'utilisation                                       |    Peut afficher                      |    Peut afficher                      |    Peut afficher                      |    Peut afficher                |    Aucun accès              |    Aucun accès                     |
|    Compte de revenu                                     |    Peut mettre à jour                    |    Aucun accès                     |    Aucun accès                     |    Aucun accès               |    Peut afficher               |    Aucun accès                     |
|    Profil fiscal                                        |    Peut mettre à jour                    |    Aucun accès                     |    Aucun accès                     |    Aucun accès               |    Peut afficher               |    Aucun accès                     |
|    Résumé du paiement                                     |    Peut afficher                      |    Aucun accès                     |    Aucun accès                     |    Aucun accès               |    Peut afficher               |    Aucun accès                     |

Si aucun des rôles standard ne convient, ou que vous souhaitez limiter l’accès à des applications et/ou extensions spécifiques, vous pouvez accorder des autorisations personnalisées à l’utilisateur en sélectionnant **Personnaliser les autorisations**, comme décrit ci-dessous.


<span id="custom" />

## <a name="assign-custom-permissions-to-account-users"></a>Attribuer des autorisations personnalisées aux utilisateurs de compte

Pour attribuer des autorisations personnalisées plutôt que des rôles standard, cliquez sur **Personnaliser les autorisations** dans la section **Rôles** lors de l’ajout ou de la modification du compte d’utilisateur. 

Pour activer une autorisation pour l’utilisateur, activez la case du paramètre approprié. 

![Guide des paramètres d’accès](images/permission_key.png)

- **Aucun accès**: L’utilisateur n’aura pas l’autorisation indiquée.
- **En lecture seule**: L’utilisateur aura accès à afficher les fonctionnalités liées à la zone indiquée, mais ne pourrez pas apporter des modifications. 
- **En lecture/écriture**: L’utilisateur aura accès à apporter des modifications associé à la zone, mais aussi afficher.
- **Mixte**: Vous ne pouvez pas sélectionner cette option directement, mais la **mixte** indicateur affichera si vous avez autorisé une combinaison d’accès pour cette autorisation. Par exemple, si vous accordez un accès **en lecture seule** à **Tarification et disponibilité** pour **Tous les produits**, mais que vous accordez ensuite un accès **en lecture/écriture** à **Tarification et disponibilité** pour un produit spécifique, l’indicateur **Tarification et disponibilité** pour **Tous les produits** sera considéré comme Mixte. Le même raisonnement s’applique si certains produits n’ont **aucun accès** pour une autorisation, mais que d’autres ont un accès **en lecture/écriture** et/ou **en lecture seule**.

Pour certaines autorisations, telles que celles liées à l’affichage des données analytiques, seul un accès **en lecture seule** peut être accordé. Notez que dans l’implémentation actuelle, certaines autorisations ne font pas la distinction entre l’accès **en lecture seule** et l’accès **en lecture/écriture**. Passez en revue les détails de chaque autorisation pour comprendre les fonctionnalités spécifiques accordées par l’accès **en lecture seule** et/ou **en lecture/écriture**.

Les informations spécifiques à chaque autorisation figurent dans les tableaux ci-dessous.

## <a name="account-level-permissions"></a>Autorisations au niveau du compte

Les autorisations de cette section ne peuvent pas être limitées à des produits donnés. En ayant accès à l’une de ces autorisations, l’utilisateur peut bénéficier de cette autorisation pour l’ensemble du compte.

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Nom de l’autorisation</th>
    <th align="left">Lecture seule</th>
    <th align="left">Lecture/écriture</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    <b>Paramètres de compte</b>                    </td><td align="left">  Peut afficher toutes les pages de la section <b>Paramètres du compte</b>, y compris les <a href="managing-your-profile.md">coordonnées</a>.       </td><td align="left">  Peut afficher toutes les pages de la section <b>Paramètres du compte</b>. Peut modifier les <a href="managing-your-profile.md">coordonnées</a> et d’autres pages, mais ne peut pas apporter de modifications au compte de revenu ou au profil fiscal (à moins que l’autorisation ne soit accordée séparément).            </td></tr>
<tr><td align="left">    <b>Utilisateurs de comptes</b>                       </td><td align="left">  Peut afficher les utilisateurs qui ont été ajoutés au compte dans la section <b>Utilisateurs</b>.          </td><td align="left">  Peut ajouter des utilisateurs au compte et modifier les utilisateurs existants dans la section <b>Utilisateurs</b>.             </td></tr>
<tr><td align="left">    <b>Rapport de performances au niveau du compte ad</b> </td><td align="left">  Peut afficher le <a href="advertising-performance-report.md">Rapport sur les performances publicitaires</a> au niveau du compte.      </td><td align="left">  N/A   </td></tr>
<tr><td align="left">    <b>Campagnes de publicité</b>                        </td><td align="left">  Peut afficher les <a href="create-an-ad-campaign-for-your-app.md">campagnes publicitaires</a> créées dans le compte.      </td><td align="left">  Peut créer, gérer et afficher les <a href="create-an-ad-campaign-for-your-app.md">campagnes publicitaires</a> créées dans le compte.          </td></tr>
<tr><td align="left">    <b>Médiation d’AD</b>                        </td><td align="left">  Peut afficher les configurations de médiation ad pour tous les produits dans le compte.    </td><td align="left">  Peut afficher et modifier les configurations de médiation ad pour tous les produits dans le compte.        </td></tr>
<tr><td align="left">    <b>Rapports de médiation AD</b>                </td><td align="left">  Peut afficher le <a href="ad-mediation-report.md">rapport de médiation publicitaire</a> de tous les produits dans le compte.    </td><td align="left">  N/A    </td></tr>
<tr><td align="left">    <b>Rapports de performances AD</b>              </td><td align="left">  Peut afficher les <a href="advertising-performance-report.md">rapports sur les performances publicitaires</a> de tous les produits dans le compte.       </td><td align="left">  N/A         </td></tr>
<tr><td align="left">    <b>Unités d’AD</b>                            </td><td align="left">  Peut afficher les <a href="in-app-ads.md">unités publicitaires</a> qui ont été créées pour le compte.    </td><td align="left">  Peut créer, gérer et afficher les <a href="in-app-ads.md">unités publicitaires</a> du compte.             </td></tr>
<tr><td align="left">    <b>Applications associées à des annonces</b>                       </td><td align="left">  Peut afficher l’utilisation de l’<a href="about-affiliate-ads.md">annonce des affiliés</a> dans tous les produits du compte.    </td><td align="left">  Peut gérer et afficher l’utilisation de l’<a href="about-affiliate-ads.md">annonce des affiliés</a> dans tous les produits du compte.                </td></tr>
<tr><td align="left">    <b>Rapports de performances de filiales</b>      </td><td align="left">  Peut afficher le <a href="affiliates-performance-report.md">rapport sur les performances des annonces des affiliés</a> de tous les produits dans le compte.   </td><td align="left">  N/A   </td></tr>
<tr><td align="left">    <b>Rapports de publicités install application</b>             </td><td align="left">  Peut visualiser le <a href="promote-your-app-report.md">rapport de campagne de publicité</a>.           </td><td align="left">  N/A   </td></tr>
<tr><td align="left">    <b>Annonces de communauté</b>                       </td><td align="left">  Peut afficher l’utilisation des <a href="about-community-ads.md">annonces gratuites de la communauté</a> de tous les produits dans le compte.          </td><td align="left">  Peut créer, gérer et afficher l’utilisation des <a href="about-community-ads.md">annonces gratuites de la communauté</a> de tous les produits dans le compte.               </td></tr>
<tr><td align="left">    <b>Informations de contact</b>                        </td><td align="left">  Peut afficher les <a href="managing-your-profile.md">coordonnées</a> dans la section Paramètres du compte.        </td><td align="left">  Peut modifier et afficher les <a href="managing-your-profile.md">coordonnées</a> dans la section Paramètres du compte.            </td></tr>
<tr><td align="left">    <b>Conformité COPPA</b>                    </td><td align="left">  Peut afficher les sélections de <a href="in-app-ads.md#coppa-compliance">conformité avec la réglementation COPPA</a> (qui indique si les produits sont adaptés aux enfants de moins de 13 ans) pour tous les produits dans le compte.                                            </td><td align="left">  Peut modifier et afficher les sélections de <a href="in-app-ads.md#coppa-compliance">conformité avec la réglementation COPPA</a> (qui indique si les produits sont adaptés aux enfants de moins de 13 ans) pour tous les produits dans le compte.         </td></tr>
<tr><td align="left">    <b>Groupes de clients</b>                     </td><td align="left">  Peut afficher <a href="create-customer-groups.md">les groupes de clients</a> (segments et groupes d’utilisateurs connus).      </td><td align="left">  Peut créer, modifier et afficher <a href="create-customer-groups.md">les groupes de clients</a> (segments et groupes d’utilisateurs connus).       </td></tr>
<tr><td align="left">    <b>Gérer les groupes de produits</b>&nbsp;*                            </td><td align="left">  Peut afficher la page de création d’un nouveau groupe de produits, mais ne peut pas créer de nouveaux groupes de produits.    </td><td align="left">  Peut créer et modifier des groupes de produits.     </td></tr>
<tr><td align="left">    <b>Nouvelles applications</b>                            </td><td align="left">  Peut afficher la page de création d’une nouvelle application, mais ne peut pas créer de nouvelles applications dans le compte.    </td><td align="left">  Peut <a href="create-your-app-by-reserving-a-name.md">créer de nouvelles applications</a> dans le compte en réservant les noms d’application et peut créer des soumissions et envoyer des applications dans le Windows Store.     </td></tr>
<tr><td align="left">    <b>Nouveaux ensembles</b>&nbsp;*                       </td><td align="left">  Peut afficher la page de création de nouveaux ensembles, mais ne peut pas créer de nouveaux ensembles dans le compte.     </td><td align="left">  Peut créer de nouveaux ensembles de produits.          </td></tr>
<tr><td align="left">    <b>Services partenaires</b>&nbsp;*                  </td><td align="left">  Peut afficher les certificats pour l’installation de services permettant de récupérer des XTokens.     </td><td align="left">  Peut gérer et afficher les certificats pour l’installation de services permettant de récupérer des XTokens.       </td></tr>
<tr><td align="left">    <b>Compte de paiement</b>                      </td><td align="left">  Peut afficher les <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">informations sur le compte de revenu</a> dans <b>Paramètres du compte</b>.     </td><td align="left">  Peut modifier et afficher les <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">informations sur le compte de revenu</a> dans <b>Paramètres du compte</b>.       </td></tr>
<tr><td align="left">    <b>Résumé de paiement</b>                      </td><td align="left">  Peut afficher le <a href="payout-summary.md">résumé du paiement</a> pour accéder aux informations des rapports sur les paiements et les télécharger.       </td><td align="left">  Peut afficher le <a href="payout-summary.md">résumé du paiement</a> pour accéder aux informations des rapports sur les paiements et les télécharger.   </td></tr>
<tr><td align="left">    <b>Parties de confiance</b>&nbsp;*                   </td><td align="left">  Peut afficher les parties de confiance pour récupérer les XTokens.    </td><td align="left">  Peut gérer et afficher les parties de confiance pour récupérer les XTokens.     </td></tr>
<tr><td align="left">    <b>Sandboxes</b>&nbsp;*                         </td><td align="left">  Peut accéder à la page <b>Sandboxes</b> et afficher les sandboxes du compte et toutes les configurations qui s’y rapportent. Ne peut pas afficher les produits et soumissions de chaque sandbox, sauf si les autorisations appropriées au niveau du produit sont accordées. </td><td align="left">  Peut accéder à la page <b>Sandboxes</b>, et afficher et gérer les sandboxes du compte, y compris créer et supprimer des sandboxes, et gérer leur configuration. Ne peut pas afficher les produits et soumissions de chaque sandbox, sauf si les autorisations appropriées au niveau du produit sont accordées.    </td></tr>
<tr><td align="left">    <b>Événements de vente du Store</b>&nbsp;*                            </td><td align="left">  N/A    </td><td align="left">  Peut configurer la possibilité d’inclure automatiquement les produits dans les événements de vente du Store.     </td></tr>
<tr><td align="left">    <b>Profil fiscal</b>                         </td><td align="left">  Peut afficher les <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">formulaires et informations du profil fiscal</a> dans <b>Paramètres du compte</b>.     </td><td align="left">  Peut remplir les déclarations fiscales et mettre à jour les <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">informations du profil fiscal</a> dans <b>Paramètres du compte</b>.     </td></tr>
<tr><td align="left">    <b>Comptes de test</b>&nbsp;*                     </td><td align="left">  Peut afficher les comptes permettant de tester la configuration Xbox Live.      </td><td align="left">  Peut créer, gérer et afficher les comptes permettant de tester la configuration Xbox Live.      </td></tr>
<tr><td align="left">    <b>Appareils de Xbox</b>                        </td><td align="left">  Peut afficher les consoles de développement Xbox activées pour le compte dans la section <b>Paramètres du compte</b>.       </td><td align="left">  Peut ajouter, supprimer et afficher les consoles de développement Xbox activées pour le compte dans la section <b>Paramètres du compte</b>.     </td></tr>
    </tbody>
    </table>

\* Autorisations marquée avec un astérisque (*) accorder l’accès à des fonctionnalités qui ne sont pas disponibles pour tous les comptes. Si ces fonctionnalités n’ont pas été activées pour votre compte, vos sélections concernant ces autorisations n’auront aucun effet.   


## <a name="product-level-permissions"></a>Autorisations au niveau du produit

Les autorisations de cette section peuvent être accordées à tous les produits du compte, ou personnalisées pour appliquer l’autorisation uniquement à un ou plusieurs produits spécifiques. 

Autorisations au niveau du produit sont regroupées en quatre catégories : **Analytique**, **monétisation**, **publication**, et **Xbox Live**. Vous pouvez développer chacune de ces catégories pour en afficher les autorisations individuelles. Vous avez également la possibilité d’activer **Toutes les autorisations** pour un ou plusieurs produits spécifiques.

Pour accorder une autorisation pour chaque produit du compte, effectuez vos sélections pour cette autorisation (en cochant la case **Lecture seule** ou **Lecture/écriture**) dans la ligne **Tous les produits**. 
 
> [!TIP]
> Les sélections effectuées pour **Tous les produits** s’appliquent à chaque produit figurant actuellement dans le compte, ainsi qu’à tous les produits qui seront créés plus tard dans le compte. Pour éviter que les autorisations s’appliquent aux futurs produits, sélectionnez tous les produits individuellement au lieu de choisir **Tous les produits**.

Sur la ligne **Tous les produits**, vous verrez chaque produit du compte répertorié sur une ligne distincte. Pour accorder une autorisation pour seulement un produit donné, effectuez vos sélections pour cette autorisation dans la ligne du produit en question.

Chaque module complémentaire est répertorié dans une ligne distincte sous son produit parent, ainsi que dans la ligne **Tous les modules complémentaires**. Les sélections effectuées pour **tous les modules complémentaires** s’appliquent à tous les modules complémentaires actuels de ce produit, ainsi qu’à tous les modules complémentaires qui seront créés plus tard pour ce produit.

Remarque : certaines autorisations ne peuvent pas être définies pour les modules complémentaires. Cela peut être parce qu’elles ne s’appliquent pas aux modules complémentaires (par exemple, l’autorisation **Retour d’expérience du client**) ou parce que l’autorisation accordée au niveau du produit parent s’applique à tous les modules complémentaires de ce produit (par exemple, **Codes promotionnels**). Notez toutefois que toutes les autorisations disponibles pour les modules complémentaires doivent être définies séparément ; les modules complémentaires n’héritent pas des sélections effectuées pour le produit parent. Par exemple, si vous souhaitez permettre à l’utilisateur d’effectuer des sélections de tarification et de disponibilité pour un module complémentaire, vous devez activer l’autorisation **Tarification et disponibilité** pour le module complémentaire (ou pour **Tous les modules complémentaires**), que vous ayez ou non accordé l’autorisation **Tarification et disponibilité** pour le produit parent. 


### <a name="analytics"></a>Analyses

<table>
    <thead>
    <tr class="header">
    <th align="left">Nom&nbsp;de l’autorisation</th>
    <th align="left">Lecture&nbsp;seule</th>
    <th align="left">Lecture/écriture</th>
    <th align="left">Lecture&nbsp;seule&nbsp;(module complémentaire) </th>
    <th align="left">Lecture&#8209;écriture&nbsp;(module complémentaire)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Acquisitions</b> (y compris les données quasiment en temps réel) </td><td>    Peut afficher les rapports <a href="acquisitions-report.md">Acquisitions</a> et <a href="add-on-acquisitions-report.md">Acquisitions de modules complémentaires</a> pour le produit.        </td><td>    N/A    </td><td>    N/a (paramètres pour le produit parent incluent le **acquisitions de module complémentaire** rapport)        </td><td>    N/A                         </td></tr>
    <tr><td align="left">    <b>Utilisation</b> </td><td>    Peut afficher le <a href="usage-report.md">rapport d’utilisation</a> du produit.     </td><td>    N/A       </td><td>    N/A     </td><td>    N/A         </td></tr>
    <tr><td align="left">    <b>Contrôle d’intégrité</b> (y compris les données quasiment en temps réel) </td><td>    Peut afficher le <a href="health-report.md">rapport d’intégrité</a> du produit.    </td><td>    N/A     </td><td>    N/A     </td><td>    N/A         </td></tr>
    <tr><td align="left">    <b>Commentaires des clients</b>    </td><td>    Peut visualiser les rapports <a href="reviews-report.md">Avis</a> et <a href="feedback-report.md">Commentaires</a> concernant le produit.       </td><td>    Non applicable (pour répondre à des commentaires ou à des avis, l’autorisation <b>Contacter le client</b> doit être accordée)   </td><td>    N/A     </td><td>    N/A         </td></tr>
    <tr><td align="left">    <b>Analytique de Xbox</b> </td><td>    Peut afficher le <a href="xbox-analytics-report.md">rapport d’analytique de Xbox</a> pour le produit.    </td><td>    N/A   </td><td>    N/A       </td><td>    N/A          </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>Monétisation

<table>
    <thead>
    <tr class="header">
    <th align="left">Nom&nbsp;de l’autorisation</th>
    <th align="left">Lecture&nbsp;seule</th>
    <th align="left">Lecture/écriture</th>
    <th align="left">Lecture&nbsp;seule&nbsp;(module complémentaire) </th>
    <th align="left">Lecture&#8209;écriture&nbsp;(module complémentaire)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Code promotionnel</b>     </td><td>    Peut afficher les commandes avec un <a href="generate-promotional-codes.md">code promotionnel</a> et les informations d’utilisation du produit et de ses modules complémentaires, et peut afficher les informations d’utilisation.         </td><td>    Peut afficher, gérer et créer des commandes avec un <a href="generate-promotional-codes.md">code promotionnel</a> pour le produit et pour ses modules complémentaires, et peut afficher les informations d’utilisation.          </td><td>    Non applicable (les paramètres du produit parent s’appliquent à tous les modules complémentaires)     </td><td>    Non applicable (les paramètres du produit parent s’appliquent à tous les modules complémentaires)     </td></tr>
    <tr><td align="left">    <b>Offres ciblées</b>     </td><td>    Peut visualiser les <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">offres ciblées</a> concernant le produit.         </td><td>    Peut visualiser, gérer et créer des <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">offres ciblées</a> concernant le produit.          </td><td>    N/A     </td><td>    N/A      </td></tr>
    <tr><td align="left">    <b>Contactez le</b>  </td><td>    Peut afficher les <a href="respond-to-customer-feedback.md">réponses aux commentaires des clients</a> et les <a href="respond-to-customer-reviews.md">réponses aux avis des clients</a>, à condition que l’autorisation <b>Retour d’expérience du client</b> ait également été accordée. Peut également afficher les <a href="send-push-notifications-to-your-apps-customers.md">notifications ciblées</a> qui ont été créées pour le produit.    </td><td>    Peut <a href="respond-to-customer-feedback.md">répondre aux commentaires des clients</a> et <a href="respond-to-customer-reviews.md">répondre aux revues client</a>, à condition que le <b>les commentaires des clients</b> autorisation a été accordée ainsi. Peut également <a href="send-push-notifications-to-your-apps-customers.md">créer et envoyer des notifications ciblées</a> pour le produit.                   </td><td>    N/A         </td><td>    N/A                          </td></tr>
    <tr><td align="left">    <b>Expérimentation</b></td><td>    Peut afficher les <a href="../monetize/run-app-experiments-with-a-b-testing.md">expériences (test A/B)</a> et consulter les données de l’expérimentation du produit.   </td><td>    Peut créer, gérer et afficher les <a href="../monetize/run-app-experiments-with-a-b-testing.md">expériences (test A/B)</a> pour le produit, et consulter les données d’expérimentation.     </td><td>    N/A  </td><td>    N/A                 </td></tr>
    <tr><td align="left">    <b>Événements de vente du Store</b>&nbsp;*</td><td>    Peut afficher l’état de l’événement de vente pour le produit.   </td><td>    Peut ajouter le produit à des événements de vente et configurer des remises.      </td><td>    Peut afficher l’état de l’événement de vente pour le produit.   </td><td>    Peut ajouter le produit à des événements de vente et configurer des remises.      </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>Publication 

<table>
    <thead>
    <tr class="header">
    <th align="left">Nom&nbsp;de l’autorisation</th>
    <th align="left">Lecture&nbsp;seule</th>
    <th align="left">Lecture/écriture</th>
    <th align="left">Lecture&nbsp;seule&nbsp;(module complémentaire) </th>
    <th align="left">Lecture&#8209;écriture&nbsp;(module complémentaire)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Programme d’installation du produit</b>  </td><td>    Peut afficher la page d’installation du produit de produits.     </td><td>    Peut afficher et modifier la page d’installation du produit de produits. </td><td>    Peut afficher la page d’installation du produit des modules complémentaires.   </td><td>    Peut afficher et modifier les modules complémentaires page de produit le programme d’installation.          </td></tr>
    <tr><td align="left">    <b>Tarification et disponibilité</b>  </td><td>    Peut afficher le <a href="set-app-pricing-and-availability.md">tarification et disponibilité</a> page de produits.     </td><td>    Peut afficher et modifier le <a href="set-app-pricing-and-availability.md">tarification et disponibilité</a> page de produits. </td><td>    Peut afficher le <a href="set-add-on-pricing-and-availability.md">tarification et disponibilité</a> page des modules complémentaires.   </td><td>    Peut afficher et modifier le <a href="set-add-on-pricing-and-availability.md">tarification et disponibilité</a> page des modules complémentaires.          </td></tr>
    <tr><td align="left">    <b>Propriétés</b>   </td><td>    Peut afficher le <a href="enter-app-properties.md">propriétés</a> page de produits.      </td><td>    Peut afficher et modifier le <a href="enter-app-properties.md">propriétés</a> page de produits.       </td><td>    Peut afficher le <a href="enter-add-on-properties.md">propriétés</a> page des modules complémentaires.     </td><td>    Peut afficher et modifier le <a href="enter-add-on-properties.md">propriétés</a> page des modules complémentaires.               </td></tr>
    <tr><td align="left">    <b>Classes d’âge</b>    </td><td>    Peut afficher le <a href="age-ratings.md">âge évaluations</a> page de produits.       </td><td>    Peut afficher et modifier le <a href="age-ratings.md">âge évaluations</a> page de produits.    </td><td>    Peut afficher la page de contrôle d’accès d’âge des modules complémentaires.          </td><td>     Peut afficher et modifier la page de contrôle d’accès d’âge des modules complémentaires.       </td></tr>
    <tr><td align="left">    <b>Packages</b>        </td><td>    Peut afficher le <a href="upload-app-packages.md">Packages</a> page de produits.  </td><td>    Peut afficher et modifier le <a href="upload-app-packages.md">Packages</a> page de produits, notamment de télécharger les packages.     </td><td>   Peut afficher le <a href="upload-app-packages.md">Packages</a> page des modules complémentaires (le cas échéant).   </td><td>     Peut afficher et modifier <a href="upload-app-packages.md">Packages</a> page des modules complémentaires (le cas échéant).             </td></tr>
    <tr><td align="left">    <b>Listes de Store</b>  </td><td>    Peut afficher le <a href="create-app-store-listings.md">Store listing page (s)</a> de produits.  </td><td>    Peut afficher et modifier le <a href="create-app-store-listings.md">Store listing page (s)</a> de produits et pouvez ajouter de nouvelles listes de Store pour des langues différentes.     </td><td>    Peut afficher le <a href="create-add-on-store-listings.md">Store listing page (s)</a> des modules complémentaires.            </td><td>    Peut afficher et modifier le <a href="create-add-on-store-listings.md">Store listing page (s)</a> des modules complémentaires et peut ajouter des annonces de Store pour des langues différentes.                 </td></tr>
    <tr><td align="left">    <b>Soumission de Store</b>     </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.           </td><td>    Peut soumettre le produit sur le Windows Store et consulter les rapports de certification. Inclut les nouvelles soumissions et les soumissions mises à jour. </td><td>Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.     </td><td>    Peut soumettre le module complémentaire sur le Windows Store et consulter les rapports de certification. Inclut les nouvelles soumissions et les soumissions mises à jour.</td></tr>
    <tr><td align="left">    <b>Création d’un dépôt</b>       </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.        </td><td>    Peut créer de nouvelles <a href="app-submissions.md">soumissions</a> du produit.  </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.   </td><td>    Peut créer de nouvelles <a href="add-on-submissions.md">soumissions</a> du module complémentaire.        </td></tr>
    <tr><td align="left">    <b>Nouveaux modules complémentaires</b>    </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule. </td><td>    Peut <a href="set-your-add-on-product-id.md">créer de nouveaux modules complémentaires</a> pour le produit. </td><td>    N/A    </td><td>    N/A        </td></tr>
    <tr><td align="left">    <b>Réservations de nom</b>   </td><td>    Peut afficher la page <a href="manage-app-names.md">Gestion des noms d’application</a> pour le produit.</td><td>    Peut afficher et modifier la page <a href="manage-app-names.md">Gestion des noms d’application</a> pour le produit, y compris réserver des noms supplémentaires et supprimer des noms réservés. </td><td>   Peut afficher les noms réservés pour le module complémentaire.    </td><td>   Peut afficher et modifier les noms réservés pour le module complémentaire.          </td></tr>
    <tr><td align="left">    <b>Demande de disque</b>   </td><td>    Peut afficher la page demande du disque. </td><td>    Peut créer des demandes de disque. </td><td>   N/A    </td><td>   N/A          </td></tr>
    <tr><td align="left">    <b>Droits d’auteur disque </b>   </td><td>    Peut afficher la page de droits d’auteur du disque.</td><td>    Peut créer des droits d’auteur disque. </td><td>   N/A    </td><td>   N/A          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">Nom&nbsp;de l’autorisation</th>
    <th align="left">Lecture&nbsp;seule</th>
    <th align="left">Lecture/écriture</th>
    <th align="left">Lecture&nbsp;seule&nbsp;(module complémentaire) </th>
    <th align="left">Lecture&#8209;écriture&nbsp;(module complémentaire)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Parties de confiance</b>&nbsp;*</td><td>    Peut afficher la page de parties de partie de confiance d’un compte.   </td><td>    Peut afficher et modifier la page de parties de partie de confiance d’un compte.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Services partenaires</b>&nbsp;*</td><td>    Peut afficher la page de services Web d’un compte.  </td><td>    Peut afficher et modifier la page de services Web d’un compte.      </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Comptes de Test Xbox</b>&nbsp;*</td><td>    Peut afficher la page comptes de Test Xbox d’un compte.  </td><td>    Peut afficher et modifier la page comptes de Test Xbox d’un compte.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Comptes de Test de Xbox par le bac à sable</b>&nbsp;*</td><td>    Peut afficher la page comptes de Test Xbox pour uniquement les bacs à sable spécifiés d’un compte.  </td><td>    Peut afficher et modifier le Test de la Xbox.   <tr><td align="left">    <b>Page de comptes pour seulement les bacs à sable spécifiés d’un compte    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Appareils de Xbox</b>&nbsp;*</td><td>    Peut afficher la page de consoles Xbox un développement d’un compte.  </td><td>    Peut afficher et modifier la page de consoles Xbox un développement d’un compte.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Appareils de Xbox par le bac à sable</b>&nbsp;*</td><td>    Peut afficher la page de consoles Xbox un développement pour seulement les bacs à sable spécifiés d’un compte.  </td><td>    Peut afficher et modifier la page de consoles Xbox un développement pour seulement les bacs à sable spécifiés d’un compte.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Canaux d’application</b>&nbsp;*</td><td>    N/A  </td><td>    Peut publier des canaux de vidéos promotionnelles sur la console Xbox, à afficher via OneGuide.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Configuration du service</b>&nbsp;*</td><td>    Peut afficher la page de configuration de Service Xbox Live d’un produit.  </td><td>    Peut afficher et modifier la page de configuration de Service Xbox Live d’un produit.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Accès aux outils</b>&nbsp;*</td><td>    Peut exécuter les outils Xbox Live sur un produit pour afficher uniquement les données.  </td><td>    Peut exécuter les outils Xbox Live sur un produit pour afficher et modifier des données.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
</tbody>
</table>

\* Autorisations marquée avec un astérisque (*) accorder l’accès à des fonctionnalités qui ne sont pas disponibles pour tous les comptes. Si ces fonctionnalités n’ont pas été activées pour votre compte, vos sélections concernant ces autorisations n’auront aucun effet.  
