---
author: jnHs
Description: Set custom permissions for account users.
title: "Définir des autorisations personnalisées pour les utilisateurs de compte"
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, rôles d’utilisateur, autorisation d’utilisateur, rôles personnalisés, accès utilisateur, personnaliser les autorisations, rôles standard"
ms.localizationpriority: high
ms.openlocfilehash: 1fdde4be606abae849ff3350d27afbbced157f75
ms.sourcegitcommit: 446fe2861651f51a129baa80791f565f81b4f317
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/12/2018
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>Définir des rôles ou des autorisations personnalisés pour les utilisateurs de compte

Lorsque vous [ajoutez des utilisateurs à votre compte du Centre de développement](add-users-groups-and-azure-ad-applications.md), vous devez spécifier l’accès dont ils disposent dans ce compte. Pour effectuer cette opération, vous pouvez attribuer aux utilisateurs des [rôles standard](#roles) qui s’appliquent à la totalité du compte, ou vous pouvez [personnaliser leurs autorisations](#custom) afin de leur fournir le niveau d’accès approprié. Certaines des autorisations personnalisées s’appliquent à l’ensemble du compte, tandis que d’autres peuvent être limitées à un ou plusieurs produits spécifiques (ou accordées à tous les produits si vous préférez ce cas de figure).

> [!NOTE] 
> Vous pouvez appliquer les mêmes rôles et autorisations, que vous ajoutiez un utilisateur, un groupe ou une application AzureAD.

Lorsque vous déterminez le rôle ou les autorisations à appliquer, gardez à l’esprit les points suivants: 
-   Les utilisateurs (y compris les groupes et applications AzureAD) pourront accéder à l’ensemble du compte du Centre de développement avec les autorisations associées au rôle qui leur est attribué, sauf si vous [personnalisez les autorisations](#custom) et attribuez des [autorisations au niveau du produit](#product-level-permissions) afin que les utilisateurs puissent utiliser uniquement des applications et/ou des extensions spécifiques.
-   Vous pouvez autoriser un utilisateur, un groupe ou une application Azure AD à accéder aux fonctionnalités de différents rôles en sélectionnant plusieurs rôles ou en utilisant les autorisations personnalisées pour accorder l’accès de votre choix.
-   Un utilisateur ayant un certain rôle (ou un ensemble d’autorisations personnalisées) peut également faire partie d’un groupe ayant un rôle différent (ou un ensemble d’autorisations). Dans ce cas, l’utilisateur a accès à toutes les fonctionnalités associées à la fois au groupe et au compte individuel.

> [!TIP]
> Cette rubrique est spécifique au programme destiné aux développeurs d’applications Windows. Pour plus d’informations sur les rôles utilisateurs dans le programme pour développeurs de matériel, voir [Gestion des rôles d’utilisateurs](https://docs.microsoft.com/windows-hardware/drivers/dashboard/managing-user-roles).


<span id="roles" />
## <a name="assign-roles-to-account-users"></a>Attribuer des rôles aux utilisateurs de compte

Par défaut, un ensemble de rôles standard vous est présenté pour vous permettre d’effectuer une sélection lorsque vous ajoutez un utilisateur, un groupe ou une application AzureAD à votre compte du Centre de développement. Chaque rôle dispose d’un ensemble d’autorisations spécifique lui permettant d’exécuter certaines fonctions dans le cadre du compte. 

À moins que vous ne choisissiez de définir des [autorisations personnalisées](#custom) en sélectionnant **Personnaliser les autorisations**, chaque utilisateur, groupe ou application AzureAD que vous ajoutez à un compte doit se voir attribuer au moins l’un des rôles standard ci-après. 

> [!NOTE]
> Le **propriétaire** du compte est la personne qui l’a créé en premier avec un compte Microsoft (et non l’un des utilisateurs ajoutés par le biais d’AzureAD). Il est le seul à disposer d’un accès complet au compte et à pouvoir notamment supprimer des applications, créer et modifier l’ensemble des utilisateurs du compte et modifier tous les paramètres financiers et de compte. 


| Rôle                 | Description              |
|----------------------|--------------------------|
| Manager              | Dispose d’un accès complet au compte, mais ne peut pas modifier les paramètres fiscaux et de revenus. Ceci inclut la gestion des utilisateurs dans le Centre de développement. Cependant, notez que la possibilité de créer et supprimer des utilisateurs dans le client AzureAD dépend des autorisations du compte dans Azure AD. Ainsi, si le rôle Manager est attribué à un utilisateur, mais que celui-ci ne dispose pas des autorisations d’administrateur global dans le service AzureAD de l’organisation, il ne pourra pas créer d’utilisateurs ni supprimer des utilisateurs de l’annuaire (toutefois, il pourra modifier le rôle d’un utilisateur dans le Centre de développement). <p> Notez que si le compte du Centre de développement est associé à plusieurs client AzureAD, un manager ne peut pas voir tous les détails d'un utilisateur (y compris ses prénom, nom, e-mail de récupération de mot de passe, et s’il est un administrateur global Azure AD), sauf s'il est connecté au même client que cet utilisateur avec un compte disposant des autorisations d’administrateur global pour ce client. Toutefois, il peut ajouter et supprimer des utilisateurs dans n’importe quel client qui est associé au compte du Centre de développement. |
| Développeur            | Peut charger des packages, soumettre des applications et extensions et afficher le [Rapport d’utilisation](usage-report.md) pour obtenir des informations de télémétrie détaillées. Il ne peut afficher ni les informations financières ni les paramètres de compte.   |
| Contributeur professionnel | Peut afficher des rapports [d’intégrité](health-report.md) et [d’utilisation](usage-report.md). Impossible de créer ou soumettre des produits, de modifier des paramètres de compte ou d’afficher des informations financières.                                         |
| Contributeur financier  | Peut afficher des [rapports sur les revenus](payout-summary.md), des informations financières et des rapports d’acquisition. Il ne peut apporter aucune modification aux applications, extensions et paramètres de compte.                                                                                                                                   |
| Responsable marketing             | Peut [répondre aux avis de clients](respond-to-customer-reviews.md) et afficher des [rapports analytiques](analytics.md) non financiers. Il ne peut apporter aucune modification aux applications, extensions et paramètres de compte.      |

Le tableau ci-dessous présente certaines fonctionnalités spécifiques disponibles pour chacun de ces rôles (et pour le propriétaire du compte).

|                                 |    Propriétaire du compte                 |    Responsable                       |    Développeur                     |    Contributeur professionnel    |    Contributeur financier    |    Responsable marketing                      |
|---------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    Rapport d’acquisition           |    Peut afficher                      |    Peut afficher                      |     Aucun accès                    |     Aucun accès              |    Peut afficher               |    Aucun accès                     |
|    Rapport de commentaires/réponses    |    Peut afficher et envoyer des commentaires    |    Peut afficher et envoyer des commentaires    |    Peut afficher et envoyer des commentaires    |     Aucun accès              |     Aucun accès             |    Peut afficher et envoyer des commentaires    |
|    Rapport d’intégrité                |    Peut afficher                      |    Peut afficher                      |    Peut afficher                      |    Peut afficher                |     Aucun accès             |    Aucun accès                     |
|    Rapport d’utilisation                 |    Peut afficher                      |    Peut afficher                      |    Peut afficher                      |    Peut afficher                |     Aucun accès             |    Aucun accès                     |
|    Compte de revenu               |    Peut mettre à jour                    |    Aucun accès                     |    Aucun accès                     |    Aucun accès               |    Peut afficher               |    Aucun accès                     |
|    Profil fiscal                  |    Peut mettre à jour                    |    Aucun accès                     |    Aucun accès                     |    Aucun accès               |    Peut afficher               |    Aucun accès                     |
|    Résumé du paiement               |    Peut afficher                      |    Aucun accès                     |    Aucun accès                     |    Aucun accès               |    Peut afficher               |    Aucun accès                     |

Si aucun des rôles standard ne convient, ou que vous souhaitez limiter l’accès à des applications et/ou extensions spécifiques, vous pouvez accorder des autorisations personnalisées à l’utilisateur en sélectionnant **Personnaliser les autorisations**, comme décrit ci-dessous.


<span id="custom" />
## <a name="assign-custom-permissions-to-account-users"></a>Attribuer des autorisations personnalisées aux utilisateurs de compte

Pour attribuer des autorisations personnalisées plutôt que des rôles standard, cliquez sur **Personnaliser les autorisations** dans la section **Rôles** lors de l’ajout ou de la modification du compte d’utilisateur. 

Pour activer une autorisation pour l’utilisateur, activez la case du paramètre approprié. 

![Guide des paramètres d’accès](images/permission_key.png)

- **Aucun accès**: l’utilisateur n’aura pas l’autorisation indiquée.
- **Lecture seule**: l’utilisateur pourra afficher les fonctionnalités associées à la zone indiquée, mais ne sera pas en mesure d’apporter de modifications. 
- **Lecture/écriture**: l’utilisateur pourra visualiser la zone et y apporter des modifications.
- **Mixte**: vous ne pouvez pas sélectionner cette option directement, mais l’indicateur **Mixte** montre si vous avez autorisé une combinaison d’accès pour cette autorisation. Par exemple, si vous accordez un accès **en lecture seule** à **Tarification et disponibilité** pour **Tous les produits**, mais que vous accordez ensuite un accès **en lecture/écriture** à **Tarification et disponibilité** pour un produit spécifique, l’indicateur **Tarification et disponibilité** pour **Tous les produits** sera considéré comme Mixte. Le même raisonnement s’applique si certains produits n’ont **aucun accès** pour une autorisation, mais que d’autres ont un accès **en lecture/écriture** et/ou **en lecture seule**.

Pour certaines autorisations, telles que celles liées à l’affichage des données analytiques, seul un accès **en lecture seule** peut être accordé. Notez que dans l’implémentation actuelle, certaines autorisations ne font pas la distinction entre l’accès **en lecture seule** et l’accès **en lecture/écriture**. Examinez les détails de chaque autorisation pour comprendre les fonctionnalités spécifiques accordées par l’accès **Lecture seule** et/ou **Lecture/écriture**.

Les informations propres à chaque autorisation figurent dans les tableaux ci-dessous.

## <a name="account-level-permissions"></a>Autorisations au niveau du compte

Les autorisations de cette section ne peuvent pas être limitées à des produits donnés. En ayant accès à ces autorisations, l’utilisateur peut bénéficier de cette autorisation pour l’ensemble du compte.

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
<tr><td align="left">    **Paramètres du compte**                    </td><td align="left">  Peut afficher toutes les pages de la section **Paramètres du compte**, y compris les [coordonnées](managing-your-profile.md).       </td><td align="left">  Peut afficher toutes les pages de la section **Paramètres du compte**. Peut modifier les [coordonnées](managing-your-profile.md) et d’autres pages, mais ne peut pas apporter de modifications au compte de revenu ou au profil fiscal (à moins que l’autorisation ne soit accordée séparément).            </td></tr>
<tr><td align="left">    **Utilisateurs de compte**                       </td><td align="left">  Peut afficher les utilisateurs qui ont été ajoutés au compte dans la section **Utilisateurs**.          </td><td align="left">  Peut ajouter des utilisateurs au compte et modifier les utilisateurs existants dans la section **Utilisateurs**.             </td></tr>
<tr><td align="left">    **Rapport sur les performances publicitaires au niveau du compte** </td><td align="left">  Peut visualiser le [rapport sur les performances publicitaires](advertising-performance-report.md) au niveau du compte.      </td><td align="left">  N/A   </td></tr>
<tr><td align="left">    **Campagnes publicitaires**                        </td><td align="left">  Peut afficher les [campagnes publicitaires](create-an-ad-campaign-for-your-app.md) créées dans le compte.      </td><td align="left">  Peut créer, gérer et afficher les [campagnes publicitaires](create-an-ad-campaign-for-your-app.md) créées dans le compte.          </td></tr>
<tr><td align="left">    **Médiation publicitaire**                        </td><td align="left">  Peut afficher les [configurations de médiation publicitaire](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) de tous les produits dans le compte.    </td><td align="left">  Peut afficher et modifier les [configurations de médiation publicitaire](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) de tous les produits dans le compte.        </td></tr>
<tr><td align="left">    **Rapports de médiation publicitaire**                </td><td align="left">  Peut afficher le [rapport de médiation publicitaire](ad-mediation-report.md) de tous les produits dans le compte.    </td><td align="left">  Non applicable    </td></tr>
<tr><td align="left">    **Rapports sur les performances publicitaires**              </td><td align="left">  Peut visualiser les [rapports sur les performances publicitaires](advertising-performance-report.md) pour tous les produits dans le compte.       </td><td align="left">  Non applicable         </td></tr>
<tr><td align="left">    **Unités publicitaires**                            </td><td align="left">  Peut afficher les [unités publicitaires](in-app-ads.md) qui ont été créées pour le compte.    </td><td align="left">  Peut créer, gérer et afficher les [unités publicitaires](in-app-ads.md) du compte.             </td></tr>
<tr><td align="left">    **Annonces des affiliés**                       </td><td align="left">  Peut afficher l’utilisation de l’[annonce des affiliés](about-affiliate-ads.md) dans tous les produits du compte.    </td><td align="left">  Peut gérer et afficher l’utilisation de l’[annonce des affiliés](about-affiliate-ads.md) dans tous les produits du compte.                </td></tr>
<tr><td align="left">    **Rapports sur les performances des annonces des affiliés**      </td><td align="left">  Peut afficher le [rapport sur les performances des annonces des affiliés](affiliates-performance-report.md) de tous les produits dans le compte.   </td><td align="left">  Non applicable   </td></tr>
<tr><td align="left">    **Rapports de publicité sur l’installation d’application**             </td><td align="left">  Peut visualiser le [rapport de campagne de publicité](promote-your-app-report.md).           </td><td align="left">  Non applicable   </td></tr>
<tr><td align="left">    **Annonces de la communauté**                       </td><td align="left">  Peut afficher l’utilisation des [annonces gratuites de la communauté](about-community-ads.md) de tous les produits dans le compte.          </td><td align="left">  Peut créer, gérer et afficher l’utilisation des [annonces gratuites de la communauté](about-community-ads.md) de tous les produits dans le compte.               </td></tr>
<tr><td align="left">    **Coordonnées**                        </td><td align="left">  Peut afficher les [coordonnées](managing-your-profile.md) dans la section Paramètres du compte.        </td><td align="left">  Peut modifier et afficher les [coordonnées](managing-your-profile.md) dans la section Paramètres du compte.            </td></tr>
<tr><td align="left">    **Conformité avec la réglementation COPPA**                    </td><td align="left">  Peut afficher les sélections de [conformité avec la réglementation COPPA](in-app-ads.md#coppa-compliance) (qui indique si les produits sont adaptés aux enfants de moins de 13ans) pour tous les produits dans le compte.                                            </td><td align="left">  Peut modifier et afficher les sélections de [conformité avec la réglementation COPPA](in-app-ads.md#coppa-compliance) (qui indique si les produits sont adaptés aux enfants de moins de 13ans) pour tous les produits dans le compte.         </td></tr>
<tr><td align="left">    **Groupes de clients**                     </td><td align="left">  Peut afficher les [groupes de clients](create-customer-groups.md) (segments et groupes de versions d’évaluation) dans la section **Clients**.      </td><td align="left">  Peut créer, modifier et afficher les [groupes de clients](create-customer-groups.md) (segments et groupes de versions d’évaluation) dans la section **Clients**.       </td></tr>
<tr><td align="left">    **Nouvelles applications**                            </td><td align="left">  Peut afficher la page de création d’une nouvelle application, mais ne peut pas créer de nouvelles applications dans le compte.    </td><td align="left">  Peut [créer de nouvelles applications](create-your-app-by-reserving-a-name.md) dans le compte en réservant les noms d’application et peut créer des soumissions et envoyer des applications dans le Windows Store.     </td></tr>
<tr><td align="left">    **Nouveaux ensembles**&nbsp;*                       </td><td align="left">  Peut afficher la page de création de nouveaux ensembles, mais ne peut pas créer de nouveaux ensembles dans le compte.     </td><td align="left">  Peut créer de nouveaux ensembles de produits.          </td></tr>
<tr><td align="left">    **Services partenaires**&nbsp;*                  </td><td align="left">  Peut afficher les certificats pour l’installation de services permettant de récupérer des XTokens.     </td><td align="left">  Peut gérer et afficher les certificats pour l’installation de services permettant de récupérer des XTokens.       </td></tr>
<tr><td align="left">    **Compte de revenu**                      </td><td align="left">  Peut afficher les [informations sur le compte de revenu](setting-up-your-payout-account-and-tax-forms.md#payout-account) dans **Paramètres du compte**.     </td><td align="left">  Peut modifier et afficher les [informations sur le compte de revenu](setting-up-your-payout-account-and-tax-forms.md#payout-account) dans **Paramètres du compte**.       </td></tr>
<tr><td align="left">    **Résumé du paiement**                      </td><td align="left">  Peut afficher le [résumé du paiement](payout-summary.md) pour accéder aux informations des rapports sur les paiements et les télécharger.       </td><td align="left">  Peut afficher le [résumé du paiement](payout-summary.md) pour accéder aux informations des rapports sur les paiements et les télécharger.   </td></tr>
<tr><td align="left">    **Parties de confiance**&nbsp;*                   </td><td align="left">  Peut visualiser les parties de confiance pour récupérer les XTokens.    </td><td align="left">  Peut gérer et visualiser les parties de confiance pour récupérer les XTokens.     </td></tr>
<tr><td align="left">    **Demande de disques**&nbsp;*                   </td><td align="left">  Peut visualiser les demandes de disque de jeu.    </td><td align="left">  Peut générer et visualiser les demandes de disque de jeu.     </td></tr>
<tr><td align="left">    **Sandboxes**&nbsp;*                         </td><td align="left">  Peut accéder à la page **Sandboxes** et visualiser les sandboxes du compte et toutes les configurations qui s’y rapportent. Ne peut pas afficher les produits et soumissions de chaque sandbox, sauf si les autorisations appropriées au niveau du produit sont accordées. </td><td align="left">  Peut accéder à la page **Sandboxes**, et afficher et gérer les sandboxes du compte, y compris créer et supprimer des sandboxes, et gérer leur configuration. Ne peut pas afficher les produits et soumissions de chaque sandbox, sauf si les autorisations appropriées au niveau du produit sont accordées.    </td></tr>
<tr><td align="left">    **Profil fiscal**                         </td><td align="left">  Peut afficher les [formulaires et informations du profil fiscal](setting-up-your-payout-account-and-tax-forms.md#tax-forms) dans **Paramètres du compte**.     </td><td align="left">  Peut remplir les déclarations fiscales et mettre à jour les [informations du profil fiscal](setting-up-your-payout-account-and-tax-forms.md#tax-forms) dans **Paramètres du compte**.     </td></tr>
<tr><td align="left">    **Comptes de test**&nbsp;*                     </td><td align="left">  Peut afficher les comptes permettant de tester la configuration Xbox Live.      </td><td align="left">  Peut créer, gérer et afficher les comptes permettant de tester la configuration Xbox Live.      </td></tr>
<tr><td align="left">    **Appareils Xbox**                        </td><td align="left">  Peut afficher les consoles de développement Xbox activées pour le compte dans la section **Paramètres du compte**.       </td><td align="left">  Peut ajouter, supprimer et afficher les consoles de développement Xbox activées pour le compte dans la section **Paramètres du compte**.     </td></tr>
    </tbody>
    </table>

\* Les autorisations marquées d’un astérisque (*) accordent l’accès à des fonctionnalités qui ne sont pas disponibles pour tous les comptes. Si ces fonctionnalités n’ont pas été activées pour votre compte, vos sélections concernant ces autorisations n’auront aucun effet.   


## <a name="product-level-permissions"></a>Autorisations au niveau du produit

Les autorisations de cette section peuvent être accordées à tous les produits du compte, ou personnalisées pour appliquer l’autorisation uniquement à un ou plusieurs produits spécifiques. 

Les autorisations au niveau du produit sont regroupées en quatre catégories: **Analytique**, **Monétisation**, **Publication** et **XboxLive**. Vous pouvez développer chacune de ces catégories pour en visualiser les autorisations individuelles. Vous avez également la possibilité d’activer **Toutes les autorisations** pour un ou plusieurs produits spécifiques.

Pour accorder une autorisation pour chaque produit du compte, effectuez vos sélections pour cette autorisation (en cochant la case **Lecture seule** ou **Lecture/écriture**) dans la ligne **Tous les produits**. 
 
> [!TIP]
> Les sélections effectuées pour **Tous les produits** s’appliquent à chaque produit figurant actuellement dans le compte, ainsi qu’à tous les produits qui seront créés plus tard dans le compte. Pour éviter que les autorisations s’appliquent aux futurs produits, sélectionnez tous les produits individuellement au lieu de choisir **Tous les produits**.

Sous la ligne **Tous les produits**, chaque produit du compte est répertorié sur une ligne distincte. Pour accorder une autorisation pour seulement un produit donné, effectuez vos sélections pour cette autorisation dans la ligne du produit en question.

Chaque extension est répertoriée dans une ligne distincte sous son produit parent, ainsi que dans la ligne **Toutes les extensions**. Les sélections effectuées pour **toutes les extensions** s’appliquent à toutes les extensions actuelles de ce produit, ainsi qu’à toutes les extensions qui seront créées plus tard pour ce produit.

Remarque: certaines autorisations ne peuvent pas être définies pour les extensions. Cela peut être parce qu’elles ne s’appliquent pas aux extensions (par exemple, l’autorisation **Retour d’expérience du client**) ou parce que l’autorisation accordée au niveau du produit parent s’applique à tous les extensions de ce produit (par exemple, **Codes promotionnels**). Notez toutefois que toutes les autorisations disponibles pour les extensions doivent être définies séparément; les extensions n’héritent pas des sélections effectuées pour le produit parent. Par exemple, si vous souhaitez permettre à l’utilisateur d’effectuer des sélections de tarification et de disponibilité pour une extension, vous devez activer l’autorisation **Tarification et disponibilité** pour l'extension (ou pour **Tous les extensions**), que vous ayez ou non accordé l’autorisation **Tarification et disponibilité** pour le produit parent. 


### <a name="analytics"></a>Analytique

<table>
    <thead>
    <tr class="header">
    <th align="left">Nom&nbsp;de l’autorisation</th>
    <th align="left">Lecture&nbsp;seule</th>
    <th align="left">Lecture/écriture</th>
    <th align="left">Lecture&nbsp;seule&nbsp;(extension) </th>
    <th align="left">Lecture&#8209;écriture&nbsp;(extension)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Acquisitions**     </td><td>    Peut afficher les rapports [Acquisitions](acquisitions-report.md) et [Acquisitions d'extensions](add-on-acquisitions-report.md) pour le produit.        </td><td>    Non applicable    </td><td>    Non applicable (les paramètres du produit parent incluent les rapports d’acquisition d'extensions)        </td><td>    Non applicable                         </td></tr>
    <tr><td align="left">    **Utilisation** </td><td>    Peut afficher le [rapport d’utilisation](usage-report.md) du produit.     </td><td>    Non applicable       </td><td>    Non applicable     </td><td>    Non applicable         </td></tr>
    <tr><td align="left">    **Intégrité** </td><td>    Peut afficher le [rapport d’intégrité](health-report.md) du produit.    </td><td>    Non applicable     </td><td>    Non applicable     </td><td>    Non applicable         </td></tr>
    <tr><td align="left">    **Retour d’expérience du client**    </td><td>    Peut visualiser les rapports [Avis](reviews-report.md) et [Commentaires](feedback-report.md) concernant le produit.       </td><td>    Non applicable (pour répondre à des commentaires ou à des avis, l’autorisation **Contacter le client** doit être accordée)   </td><td>    Non applicable     </td><td>    Non applicable         </td></tr>
    <tr><td align="left">    **Analyse Xbox** </td><td>    Peut afficher le rapport d’analyse Xbox du produit. (Remarque: ce rapport n’est pas encore disponible.)    </td><td>    Non applicable   </td><td>    Non applicable       </td><td>    Non applicable          </td></tr>
    <tr><td align="left">    **En temps réel**   </td><td>    Peut visualiser le rapport en temps réel du produit. (Remarque: pour l’instant, ce rapport n’est disponible que par le biais du [Programme Insider du Centre de développement](dev-center-insider-program.md).)      </td><td>    Non applicable   </td><td>    Non applicable     </td><td>    Non applicable                 </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>Monétisation

<table>
    <thead>
    <tr class="header">
    <th align="left">Nom&nbsp;de l’autorisation</th>
    <th align="left">Lecture&nbsp;seule</th>
    <th align="left">Lecture/écriture</th>
    <th align="left">Lecture&nbsp;seule&nbsp;(extension) </th>
    <th align="left">Lecture&#8209;écriture&nbsp;(extension)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Codes promotionnels**     </td><td>    Peut afficher les commandes avec un [code promotionnel](generate-promotional-codes.md) et les informations d’utilisation du produit et de ses extensions, et peut afficher les informations d’utilisation.         </td><td>    Peut afficher, gérer et créer des commandes avec un [code promotionnel](generate-promotional-codes.md) pour le produit et pour ses extensions, et peut afficher les informations d’utilisation.          </td><td>    Non applicable (les paramètres du produit parent s’appliquent à toutes les extensions)     </td><td>    Non applicable (les paramètres du produit parent s’appliquent à toutes les extensions)     </td></tr>
    <tr><td align="left">    **Offres ciblées**     </td><td>    Peut visualiser les [offres ciblées](use-targeted-offers-to-maximize-engagement-and-conversions.md) concernant le produit.         </td><td>    Peut visualiser, gérer et créer des [offres ciblées](use-targeted-offers-to-maximize-engagement-and-conversions.md) concernant le produit.          </td><td>    Non applicable     </td><td>    Non applicable      </td></tr>
    <tr><td align="left">    **Contacter le client**  </td><td>    Peut afficher les [réponses aux commentaires des clients](respond-to-customer-feedback.md) et les [réponses aux avis des clients](respond-to-customer-reviews.md), à condition que l’autorisation **Retour d’expérience du client** ait également été accordée. Peut également afficher les [notifications ciblées](send-push-notifications-to-your-apps-customers.md) qui ont été créées pour le produit.    </td><td>    Peut [répondre aux commentaires des clients](respond-to-customer-feedback.md) et [répondre aux avis des clients](respond-to-customer-reviews.md), à condition que l’autorisation **Retour d’expérience du client** ait également été accordée. Peut également [créer et envoyer des notifications ciblées](send-push-notifications-to-your-apps-customers.md) pour le produit.                   </td><td>    Non applicable         </td><td>    Non applicable                          </td></tr>
    <tr><td align="left">    **Expérimentation**</td><td>    Peut afficher les [expériences (test A/B)](../monetize/run-app-experiments-with-a-b-testing.md) et consulter les données de l’expérimentation du produit.   </td><td>    Peut créer, gérer et afficher les [expériences (test A/B)](../monetize/run-app-experiments-with-a-b-testing.md) pour le produit, et consulter les données d’expérimentation.     </td><td>    Non applicable  </td><td>    Non applicable                 </td></tr>

    </tbody>
    </table>

### <a name="publishing"></a>Publication 

<table>
    <thead>
    <tr class="header">
    <th align="left">Nom&nbsp;de l’autorisation</th>
    <th align="left">Lecture&nbsp;seule</th>
    <th align="left">Lecture/écriture</th>
    <th align="left">Lecture&nbsp;seule&nbsp;(extension) </th>
    <th align="left">Lecture&#8209;écriture&nbsp;(extension)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Tarification et disponibilité**  </td><td>    Peut afficher la page [Tarification et disponibilité](set-app-pricing-and-availability.md) des soumissions de produits.     </td><td>    Peut afficher et modifier la page [Tarification et disponibilité](set-app-pricing-and-availability.md) des soumissions de produits. </td><td>    Peut afficher la page [Tarification et disponibilité](set-add-on-pricing-and-availability.md) des soumissions d'extensions.   </td><td>    Peut afficher et modifier la page [Tarification et disponibilité](set-add-on-pricing-and-availability.md) des soumissions d'extensions.          </td></tr>
    <tr><td align="left">    **Propriétés**   </td><td>    Peut afficher la page [Propriétés](enter-app-properties.md) des soumissions de produits.      </td><td>    Peut afficher et modifier la page [Propriétés](enter-app-properties.md) des soumissions de produits.       </td><td>    Peut afficher la page [Propriétés](enter-add-on-properties.md) des soumissions d'extensions.     </td><td>    Peut afficher et modifier la page [Propriétés](enter-add-on-properties.md) des soumissions d'extensions.               </td></tr>
    <tr><td align="left">    **Classification par âge**    </td><td>    Peut afficher la page [Classification par âge](age-ratings.md) des soumissions de produits.       </td><td>    Peut afficher et modifier la page [Classification par âge](age-ratings.md) des soumissions de produits.    </td><td>    * Peut afficher la page Classification par âge des soumissions d'extensions.          </td><td>    * Peut afficher et modifier la page Classification par âge des soumissions d'extensions.       </td></tr>
    <tr><td align="left">    **Packages**        </td><td>    Peut afficher la page [Packages](upload-app-packages.md) des soumissions de produits.  </td><td>    Peut afficher et modifier la page [Packages](upload-app-packages.md) des soumissions de produits, y compris pour le téléchargement de packages.     </td><td>    * Peut afficher le ciblage des familles d’appareils et les packages (le cas échéant) des soumissions d'extensions.   </td><td>    * Peut afficher et modifier le ciblage des familles d’appareils des soumissions d'extensions, y compris pour le téléchargement de packages, le cas échéant.             </td></tr>
    <tr><td align="left">    **Descriptions dans le Windows Store**  </td><td>    Peut afficher la ou les [pages de description du Windows Store](create-app-store-listings.md) pour les soumissions de produits.  </td><td>    Peut afficher et modifier la ou les [pages de description du Windows Store](create-app-store-listings.md) pour les soumissions de produits, et peut ajouter de nouvelles descriptions en d’autres langues.     </td><td>    Peut afficher la ou les [pages de description du Windows Store](create-add-on-store-listings.md) pour les soumissions d'extensions.            </td><td>    Peut afficher et modifier la ou les [pages de description du Windows Store](create-add-on-store-listings.md) pour les soumissions d'extensions, et peut ajouter des descriptions en d’autres langues.                 </td></tr>
    <tr><td align="left">    **Soumission sur le Windows Store**     </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.           </td><td>    Peut soumettre le produit sur le Windows Store et consulter les rapports de certification. Inclut les nouvelles soumissions et les soumissions mises à jour. </td><td>Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.     </td><td>    Peut soumettre l'extension sur le Windows Store et consulter les rapports de certification. Inclut les nouvelles soumissions et les soumissions mises à jour.</td></tr>
    <tr><td align="left">    **Création de nouvelles soumissions**       </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.        </td><td>    Peut créer de nouvelles [soumissions](app-submissions.md) du produit.  </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.   </td><td>    Peut créer de nouvelles [soumissions](add-on-submissions.md) de l'extension.        </td></tr>
    <tr><td align="left">    **Nouvelles extensions**    </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule. </td><td>    Peut [créer de nouvelles extensions](set-your-add-on-product-id.md) pour le produit. </td><td>    Non applicable    </td><td>    Non applicable        </td></tr>
    <tr><td align="left">    **Réservation de noms**   </td><td>    Peut afficher la page [Gestion des noms d’application](manage-app-names.md) pour le produit.</td><td>    Peut afficher et modifier la page [Gestion des noms d’application](manage-app-names.md) pour le produit, y compris réserver des noms supplémentaires et supprimer des noms réservés. </td><td>   * Peut afficher les noms réservés pour l'extension.    </td><td>   * Peut afficher et modifier les noms réservés pour l'extension.          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">Nom&nbsp;de l’autorisation</th>
    <th align="left">Lecture&nbsp;seule</th>
    <th align="left">Lecture/écriture</th>
    <th align="left">Lecture&nbsp;seule&nbsp;(extension) </th>
    <th align="left">Lecture&#8209;écriture&nbsp;(extension)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Canaux d’application**&nbsp;\*</td><td>    Non applicable  </td><td>    Peut publier des canaux de vidéos promotionnelles sur la console Xbox, à afficher via OneGuide.  </td><td>  Non applicable </td><td> Non applicable </td></tr>
    <tr><td align="left">    **Configuration du service**&nbsp;\*    </td><td>    Peut visualiser les paramètres liés aux succès, au mode multijoueur, aux classements et aux autres configurations XboxLive pour le produit.  </td><td>    Peut afficher et modifier les paramètres liés aux succès, au mode multijoueur, aux classements et aux autres configurations Xbox Live pour le produit.  </td><td>    Non applicable     </td><td>    Non applicable                      </td></tr>
</tbody>
</table>

\* Les autorisations marquées d’un astérisque (*) accordent l’accès à des fonctionnalités qui ne sont pas disponibles pour tous les comptes. Si ces fonctionnalités n’ont pas été activées pour votre compte, vos sélections concernant ces autorisations n’auront aucun effet.  
