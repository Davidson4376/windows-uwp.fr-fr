---
author: jnHs
Description: "Définissez des autorisations personnalisées pour les utilisateurs de compte."
title: "Définir des autorisations personnalisées pour les utilisateurs de compte"
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 23d8c14bfdbfc05a1397fa67cb831d38ec092233
ms.lasthandoff: 02/08/2017

---

# <a name="set-custom-permissions-for-account-users"></a>Définir des autorisations personnalisées pour les utilisateurs de compte

Lorsque vous ajoutez des utilisateurs à votre compte, vous pouvez leur attribuer un [rôle standard](manage-account-users.md#roles-and-permissions), ou choisir de personnaliser leurs autorisations pour leur fournir un niveau d’accès approprié. Certaines de ces autorisations s’appliquent à l’ensemble du compte, tandis que d’autres peuvent être accordées à tous les produits ou limitées à certains produits. 

Pour utiliser des autorisations personnalisées plutôt que des rôles standard, cliquez sur **Personnaliser les autorisations** dans la section **Rôles** lors de l’ajout ou de la modification du compte d’utilisateur. 

> **Remarque** Les mêmes autorisations peuvent être appliquées, que vous ajoutiez un utilisateur, un groupe ou une application Azure AD.

Pour activer une autorisation pour l’utilisateur, activez la case du paramètre approprié. 

![Guide des paramètres d’accès](images/permission_key.png)

- **Aucun accès** : l’utilisateur n’aura pas l’autorisation indiquée.
- **Lecture seule** : l’utilisateur pourra afficher les fonctionnalités liées à la zone indiquée, mais ne pourra pas apporter de modifications.
- **Lecture/écriture** : l’utilisateur pourra visualiser la zone et y apporter des modifications.
- **Mixte** : vous ne pouvez pas sélectionner cette option directement, mais l’indicateur **Mixte** montre si vous avez autorisé une combinaison d’accès pour cette autorisation. Par exemple, si vous accordez un accès **en lecture seule** à **Tarification et disponibilité** pour **Tous les produits**, mais que vous accordez ensuite un accès **en lecture/écriture** à **Tarification et disponibilité** pour un produit spécifique, l’indicateur **Tarification et disponibilité** pour **Tous les produits** sera considéré comme Mixte. Le même raisonnement s’applique si certains produits n’ont **aucun accès** pour une autorisation, mais que d’autres ont un accès **en lecture/écriture** et/ou **en lecture seule**.

Pour certaines autorisations, telles que celles liées à l’affichage des données analytiques, seul un accès **en lecture seule** peut être accordé. Notez que dans l’implémentation actuelle, certaines autorisations ne font pas la distinction entre l’accès **en lecture seule** et l’accès **en lecture/écriture**. Passez en revue les détails de chaque autorisation pour comprendre les fonctionnalités spécifiques accordées par l’accès **en lecture seule** et **en lecture/écriture**.

Les informations spécifiques à chaque autorisation figurent dans les tableaux ci-dessous.

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
<tr><td align="left">    **Utilisateurs de compte**                       </td><td align="left">  Peut afficher les utilisateurs qui ont été ajoutés au compte dans la section **Gérer les utilisateurs**.          </td><td align="left">  Peut ajouter des utilisateurs au compte et modifier les utilisateurs existants dans la section **Gérer les utilisateurs**.             </td></tr>
<tr><td align="left">    **Rapport sur les performances publicitaires au niveau du compte** </td><td align="left">  Peut afficher le [Rapport sur les performances publicitaires](advertising-performance-report.md#account-level-advertising-performance-report) au niveau du compte. (Ne peut pas afficher les rapports sur les performances publicitaires pour chaque produit, sauf si cette autorisation est accordée séparément.)       </td><td align="left">  Non applicable   </td></tr>
<tr><td align="left">    **Campagnes publicitaires**                        </td><td align="left">  Peut afficher les [campagnes publicitaires](create-an-ad-campaign-for-your-app.md) créées dans le compte.      </td><td align="left">  Peut créer, gérer et afficher les [campagnes publicitaires](create-an-ad-campaign-for-your-app.md) créées dans le compte.          </td></tr>
<tr><td align="left">    **Médiation publicitaire**                        </td><td align="left">  Peut afficher les [configurations de médiation publicitaire](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) de tous les produits dans le compte.    </td><td align="left">  Peut afficher et modifier les [configurations de médiation publicitaire](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) de tous les produits dans le compte.        </td></tr>
<tr><td align="left">    **Rapports de médiation publicitaire**                </td><td align="left">  Peut afficher le [rapport de médiation publicitaire](ad-mediation-report.md) de tous les produits dans le compte.    </td><td align="left">  Non applicable    </td></tr>
<tr><td align="left">    **Rapports sur les performances publicitaires**              </td><td align="left">  Peut afficher les [rapports sur les performances publicitaires](advertising-performance-report.md) de tous les produits dans le compte. (Ne peut pas afficher le [rapport sur les performances publicitaires](advertising-performance-report.md#account-level-advertising-performance-report) au niveau du compte, sauf si cette autorisation est accordée séparément.)       </td><td align="left">  Peut afficher les [rapports sur les performances publicitaires](advertising-performance-report.md) de tous les produits dans le compte. (Ne peut pas afficher le [rapport sur les performances publicitaires](advertising-performance-report.md#account-level-advertising-performance-report) au niveau du compte, sauf si cette autorisation est accordée séparément.)         </td></tr>
<tr><td align="left">    **Unités publicitaires**                            </td><td align="left">  Peut afficher les [unités publicitaires](monetize-with-ads.md) qui ont été créées pour le compte.    </td><td align="left">  Peut créer, gérer et afficher les [unités publicitaires](monetize-with-ads.md) du compte.             </td></tr>
<tr><td align="left">    **Annonces des affiliés**                       </td><td align="left">  Peut afficher l’utilisation de l’[annonce des affiliés](about-affiliate-ads.md) dans tous les produits du compte.    </td><td align="left">  Peut gérer et afficher l’utilisation de l’[annonce des affiliés](about-affiliate-ads.md) dans tous les produits du compte.                </td></tr>
<tr><td align="left">    **Rapports sur les performances des annonces des affiliés**      </td><td align="left">  Peut afficher le [rapport sur les performances des annonces des affiliés](affiliates-performance-report.md) de tous les produits dans le compte.   </td><td align="left">  Non applicable   </td></tr>
<tr><td align="left">    **Rapports de publicité sur l’installation d’application**             </td><td align="left">  Peut afficher le [rapport de publicité sur l’installation d’application](app-install-ads-reports.md) de tous les produits dans le compte.           </td><td align="left">  Non applicable   </td></tr>
<tr><td align="left">    **Annonces de la communauté**                       </td><td align="left">  Peut afficher l’utilisation des [annonces gratuites de la communauté](about-community-ads.md) de tous les produits dans le compte.          </td><td align="left">  Peut créer, gérer et afficher l’utilisation des [annonces gratuites de la communauté](about-community-ads.md) de tous les produits dans le compte.               </td></tr>
<tr><td align="left">    **Coordonnées**                        </td><td align="left">  Peut afficher les [coordonnées](managing-your-profile.md) dans la section Paramètres du compte.        </td><td align="left">  Peut modifier et afficher les [coordonnées](managing-your-profile.md) dans la section Paramètres du compte.            </td></tr>
<tr><td align="left">    **Conformité avec la réglementation COPPA**                    </td><td align="left">  Peut afficher les sélections de [conformité avec la réglementation COPPA](monetize-with-ads.md#coppa-compliance) (qui indique si les produits sont adaptés aux enfants de moins de 13 ans) pour tous les produits dans le compte.                                            </td><td align="left">  Peut modifier et afficher les sélections de [conformité avec la réglementation COPPA](monetize-with-ads.md#coppa-compliance) (qui indique si les produits sont adaptés aux enfants de moins de 13 ans) pour tous les produits dans le compte.         </td></tr>
<tr><td align="left">    **Groupes de clients**                     </td><td align="left">  Peut afficher les [groupes de clients](create-customer-groups.md) (segments et groupes de versions d’évaluation) dans la section **Clients**.      </td><td align="left">  Peut créer, modifier et afficher les [groupes de clients](create-customer-groups.md) (segments et groupes de versions d’évaluation) dans la section **Clients**.       </td></tr>
<tr><td align="left">    **Nouvelles applications**                            </td><td align="left">  Peut afficher la page de création d’une nouvelle application, mais ne peut pas créer de nouvelles applications dans le compte.    </td><td align="left">  Peut [créer de nouvelles applications](create-your-app-by-reserving-a-name.md) dans le compte en réservant les noms d’application et peut créer des soumissions et envoyer des applications dans le Windows Store.     </td></tr>
<tr><td align="left">    **Nouveaux ensembles**&nbsp;*                       </td><td align="left">  Peut afficher la page de création de nouveaux ensembles, mais ne peut pas créer de nouveaux ensembles dans le compte.     </td><td align="left">  Peut créer de nouveaux ensembles de produits.          </td></tr>
<tr><td align="left">    **Services partenaires**&nbsp;*                  </td><td align="left">  Peut afficher les certificats pour l’installation de services permettant de récupérer des XTokens.     </td><td align="left">  Peut gérer et afficher les certificats pour l’installation de services permettant de récupérer des XTokens.       </td></tr>
<tr><td align="left">    **Compte de revenu**                      </td><td align="left">  Peut afficher les [informations sur le compte de revenu](setting-up-your-payout-account-and-tax-forms.md#payout-account) dans **Paramètres du compte**.     </td><td align="left">  Peut modifier et afficher les [informations sur le compte de revenu](setting-up-your-payout-account-and-tax-forms.md#payout-account) dans **Paramètres du compte**.       </td></tr>
<tr><td align="left">    **Résumé du paiement**                      </td><td align="left">  Peut afficher le [résumé du paiement](payout-summary.md) pour accéder aux informations des rapports sur les paiements et les télécharger.       </td><td align="left">  Peut afficher le [résumé du paiement](payout-summary.md) pour accéder aux informations des rapports sur les paiements et les télécharger.   </td></tr>
<tr><td align="left">    **Parties de confiance**&nbsp;*                   </td><td align="left">  Peut afficher les parties de confiance pour récupérer les XTokens.    </td><td align="left">  Peut gérer et afficher les parties de confiance pour récupérer les XTokens.     </td></tr>
<tr><td align="left">    **Sandboxes**&nbsp;*                         </td><td align="left">  Peut accéder à la page **Sandboxes** et afficher les sandboxes du compte et toutes les configurations qui s’y rapportent. Ne peut pas afficher les produits et soumissions de chaque sandbox, sauf si les autorisations appropriées au niveau du produit sont accordées. </td><td align="left">  Peut accéder à la page **Sandboxes**, et afficher et gérer les sandboxes du compte, y compris créer et supprimer des sandboxes, et gérer leur configuration. Ne peut pas afficher les produits et soumissions de chaque sandbox, sauf si les autorisations appropriées au niveau du produit sont accordées.    </td></tr>
<tr><td align="left">    **Profil fiscal**                         </td><td align="left">  Peut afficher les [formulaires et informations du profil fiscal](setting-up-your-payout-account-and-tax-forms.md#tax-forms) dans **Paramètres du compte**.     </td><td align="left">  Peut remplir les déclarations fiscales et mettre à jour les [informations du profil fiscal](setting-up-your-payout-account-and-tax-forms.md#tax-forms) dans **Paramètres du compte**.     </td></tr>
<tr><td align="left">    **Comptes de test**&nbsp;*                     </td><td align="left">  Peut afficher les comptes permettant de tester la configuration Xbox Live.      </td><td align="left">  Peut créer, gérer et afficher les comptes permettant de tester la configuration Xbox Live.      </td></tr>
<tr><td align="left">    **Appareils Xbox**                        </td><td align="left">  Peut afficher les consoles de développement Xbox activées pour le compte dans la section **Paramètres du compte**.       </td><td align="left">  Peut ajouter, supprimer et afficher les consoles de développement Xbox activées pour le compte dans la section **Paramètres du compte**.     </td></tr>
    </tbody>
    </table>

\* Les autorisations marquées d’un astérisque (*) accordent l’accès à des fonctionnalités qui ne sont pas disponibles pour tous les comptes. Si ces fonctionnalités n’ont pas été activées pour votre compte, vos sélections concernant ces autorisations n’auront aucun effet.   

## <a name="product-level-permissions"></a>Autorisations au niveau du produit

Les autorisations de cette section peuvent être accordées à tous les produits du compte, ou personnalisées pour appliquer l’autorisation uniquement à un ou plusieurs produits spécifiques. Ces autorisations sont regroupées en quatre catégories : **Analytique**, **Monétisation**, **Publication** et **Xbox Live**. Vous pouvez développer chacune de ces catégories pour en afficher les autorisations individuelles. 

Pour accorder une autorisation pour tous les produits du compte, effectuez vos sélections pour cette autorisation (en cochant la case **Lecture seule**, **Lecture/écriture** ou **Aucun accès**) dans la ligne **Tous les produits**. 
 
> **Conseil** Les sélections effectuées pour **tous les produits** s’appliquent à tous les produits actuellement dans le compte, ainsi qu’à tous les produits qui seront créés plus tard dans le compte.

Sur la ligne **Tous les produits**, vous verrez chaque produit du compte répertorié sur une ligne distincte. Pour accorder une autorisation pour seulement un produit donné, effectuez vos sélections pour cette autorisation dans la ligne du produit en question.

Chaque module complémentaire est répertorié dans une ligne distincte sous son produit parent, ainsi que dans la ligne **Tous les modules complémentaires**. Les sélections effectuées pour **tous les modules complémentaires** s’appliquent à tous les modules complémentaires actuels de ce produit, ainsi qu’à tous les modules complémentaires qui seront créés plus tard pour ce produit.

Remarque : certaines autorisations ne peuvent pas être définies pour les modules complémentaires. Cela peut être parce qu’elles ne s’appliquent pas aux modules complémentaires (par exemple, l’autorisation **Retour d’expérience du client**) ou parce que l’autorisation accordée au niveau du produit parent s’applique à tous les modules complémentaires de ce produit (par exemple, **Codes promotionnels**). Notez toutefois que toutes les autorisations disponibles pour les modules complémentaires doivent être définies séparément ; les modules complémentaires n’héritent pas des sélections effectuées pour le produit parent. Par exemple, si vous souhaitez permettre à l’utilisateur d’effectuer des sélections de tarification et de disponibilité pour un module complémentaire, vous devez activer l’autorisation **Tarification et disponibilité** pour le module complémentaire (ou pour **Tous les modules complémentaires**), que vous ayez ou non accordé l’autorisation **Tarification et disponibilité** pour le produit parent. 

### <a name="analytics"></a>Analytique

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
    <tr><td align="left">    **Acquisitions**     </td><td>    Peut afficher les rapports [Acquisitions](acquisitions-report.md) et [Acquisitions de modules complémentaires](add-on-acquisitions-report.md) pour le produit.        </td><td>    Non applicable    </td><td>    Non applicable (les paramètres du produit parent incluent les rapports d’acquisition de modules complémentaires)        </td><td>    Non applicable                         </td></tr>
    <tr><td align="left">    **Utilisation** </td><td>    Peut afficher le [rapport d’utilisation](usage-report.md) du produit.     </td><td>    Non applicable       </td><td>    Non applicable     </td><td>    Non applicable         </td></tr>
    <tr><td align="left">    **Intégrité** </td><td>    Peut afficher le [rapport d’intégrité](health-report.md) du produit.    </td><td>    Non applicable     </td><td>    Non applicable     </td><td>    Non applicable         </td></tr>
    <tr><td align="left">    **Retour d'expérience du client**    </td><td>    Peut afficher les rapports [Évaluations](ratings-report.md), [Avis](reviews-report.md) et [Commentaires](feedback-report.md) pour le produit.       </td><td>    Non applicable (pour répondre à des commentaires ou à des avis, l’autorisation **Contacter le client** doit être accordée)   </td><td>    Non applicable     </td><td>    Non applicable         </td></tr>
    <tr><td align="left">    **Analyse Xbox** </td><td>    Peut afficher le rapport d’analyse Xbox du produit. (Remarque : ce rapport n’est pas encore disponible.)    </td><td>    Non applicable   </td><td>    Non applicable       </td><td>    Non applicable          </td></tr>
    <tr><td align="left">    **En temps réel**   </td><td>    Peut afficher le rapport en temps réel du produit.       </td><td>    Non applicable   </td><td>    Non applicable     </td><td>    Non applicable                 </td></tr>
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
    <tr><td align="left">    **Contacter le client**  </td><td>    Peut afficher les [réponses aux commentaires des clients](respond-to-customer-feedback.md) et les [réponses aux avis des clients](respond-to-customer-reviews.md), à condition que l’autorisation **Retour d’expérience du client** ait également été accordée. Peut également afficher les [notifications ciblées](send-push-notifications-to-your-apps-customers.md) qui ont été créées pour le produit.    </td><td>    Peut [répondre aux commentaires des clients](respond-to-customer-feedback.md) et [répondre aux avis des clients](respond-to-customer-reviews.md), à condition que l’autorisation **Retour d’expérience du client** ait également été accordée. Peut également [créer et envoyer des notifications ciblées](send-push-notifications-to-your-apps-customers.md) pour le produit.                   </td><td>    Non applicable         </td><td>    Non applicable                          </td></tr>
    <tr><td align="left">    **Expérimentation**</td><td>    Peut afficher les [expériences (test A/B)](../monetize/run-app-experiments-with-a-b-testing.md) et consulter les données de l’expérimentation du produit.   </td><td>    Peut créer, gérer et afficher les [expériences (test A/B)](../monetize/run-app-experiments-with-a-b-testing.md) pour le produit, et consulter les données d’expérimentation.     </td><td>    Non applicable  </td><td>    Non applicable                 </td></tr>
    <tr><td align="left">    **Codes promotionnels**     </td><td>    Peut afficher les commandes avec un [code promotionnel](generate-promotional-codes.md) et les informations d’utilisation du produit et de ses modules complémentaires, et peut afficher les informations d’utilisation.         </td><td>    Peut afficher, gérer et créer des commandes avec un [code promotionnel](generate-promotional-codes.md) pour le produit et pour ses modules complémentaires, et peut afficher les informations d’utilisation.          </td><td>    Non applicable (les paramètres du produit parent s’appliquent à tous les modules complémentaires)     </td><td>    Non applicable (les paramètres du produit parent s’appliquent à tous les modules complémentaires)     </td></tr>
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
    <tr><td align="left">    **Tarification et disponibilité**  </td><td>    Peut afficher la page [Tarification et disponibilité](set-app-pricing-and-availability.md) des soumissions de produits.     </td><td>    Peut afficher et modifier la page [Tarification et disponibilité](set-app-pricing-and-availability.md) des soumissions de produits. </td><td>    Peut afficher la page [Tarification et disponibilité](set-add-on-pricing-and-availability.md) des soumissions de modules complémentaires.   </td><td>    Peut afficher et modifier la page [Tarification et disponibilité](set-add-on-pricing-and-availability.md) des soumissions de modules complémentaires.          </td></tr>
    <tr><td align="left">    **Propriétés**   </td><td>    Peut afficher la page [Propriétés](enter-app-properties.md) des soumissions de produits.      </td><td>    Peut afficher et modifier la page [Propriétés](enter-app-properties.md) des soumissions de produits.       </td><td>    Peut afficher la page [Propriétés](enter-add-on-properties.md) des soumissions de modules complémentaires.     </td><td>    Peut afficher et modifier la page [Propriétés](enter-add-on-properties.md) des soumissions de modules complémentaires.               </td></tr>
    <tr><td align="left">    **Classification par âge**    </td><td>    Peut afficher la page [Classification par âge](age-ratings.md) des soumissions de produits.       </td><td>    Peut afficher et modifier la page [Classification par âge](age-ratings.md) des soumissions de produits.    </td><td>    * Peut afficher la page Classification par âge des soumissions de modules complémentaires.          </td><td>    * Peut afficher et modifier la page Classification par âge des soumissions de modules complémentaires.       </td></tr>
    <tr><td align="left">    **Packages**        </td><td>    Peut afficher la page [Packages](upload-app-packages.md) des soumissions de produits.  </td><td>    Peut afficher et modifier la page [Packages](upload-app-packages.md) des soumissions de produits, y compris pour le téléchargement de packages.     </td><td>    * Peut afficher le ciblage des familles d’appareils et les packages (le cas échéant) des soumissions de modules complémentaires.   </td><td>    * Peut afficher et modifier le ciblage des familles d’appareils des soumissions de modules complémentaires, y compris pour le téléchargement de packages, le cas échéant.             </td></tr>
    <tr><td align="left">    **Descriptions dans le Windows Store**  </td><td>    Peut afficher la ou les [pages de description du Windows Store](create-app-store-listings.md) pour les soumissions de produits.  </td><td>    Peut afficher et modifier la ou les [pages de description du Windows Store](create-app-store-listings.md) pour les soumissions de produits, et peut ajouter de nouvelles descriptions en d’autres langues.     </td><td>    Peut afficher la ou les [pages de description du Windows Store](create-add-on-store-listings.md) pour les soumissions de modules complémentaires.            </td><td>    Peut afficher et modifier la ou les [pages de description du Windows Store](create-add-on-store-listings.md) pour les soumissions de modules complémentaires, et peut ajouter des descriptions en d’autres langues.                 </td></tr>
    <tr><td align="left">    **Soumission sur le Windows Store**     </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.           </td><td>    Peut soumettre le produit sur le Windows Store et consulter les rapports de certification. Inclut les nouvelles soumissions et les soumissions mises à jour. </td><td>Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.     </td><td>    Peut soumettre le module complémentaire sur le Windows Store et consulter les rapports de certification. Inclut les nouvelles soumissions et les soumissions mises à jour.</td></tr>
    <tr><td align="left">    **Création de nouvelles soumissions**       </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.        </td><td>    Peut créer de nouvelles [soumissions](app-submissions.md) du produit.  </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule.   </td><td>    Peut créer de nouvelles [soumissions](add-on-submissions.md) du module complémentaire.        </td></tr>
    <tr><td align="left">    **Nouveaux modules complémentaires**    </td><td>    Aucun accès n’est accordé si cette autorisation est définie sur Lecture seule. </td><td>    Peut [créer de nouveaux modules complémentaires](set-your-add-on-product-id.md) pour le produit. </td><td>    Non applicable    </td><td>    Non applicable        </td></tr>
    <tr><td align="left">    **Réservation de noms**   </td><td>    Peut afficher la page [Gestion des noms d’application](manage-app-names.md) pour le produit.</td><td>    Peut afficher et modifier la page [Gestion des noms d’application](manage-app-names.md) pour le produit, y compris réserver des noms supplémentaires et supprimer des noms réservés. </td><td>   * Peut afficher les noms réservés pour le module complémentaire.    </td><td>   * Peut afficher et modifier les noms réservés pour le module complémentaire.          </td></tr>
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
    <tr><td align="left">    **Configuration du service Xbox**&nbsp;\*    </td><td>    Peut afficher les paramètres liés aux succès, au mode multijoueur, aux classements et aux autres configurations Xbox Live pour le produit.  </td><td>    Peut afficher et modifier les paramètres liés aux succès, au mode multijoueur, aux classements et aux autres configurations Xbox Live pour le produit.  </td><td>    Non applicable     </td><td>    Non applicable                      </td></tr>
    <tr><td align="left">    **Canaux d’application**&nbsp;\*</td><td>    Non applicable  </td><td>    Peut publier des canaux de vidéos promotionnelles sur la console Xbox, à afficher via OneGuide.  </td><td>  Non applicable </td><td> Non applicable </td></tr>
</tbody>
</table>

\* Les autorisations marquées d’un astérisque (*) accordent l’accès à des fonctionnalités qui ne sont pas disponibles pour tous les comptes. Si ces fonctionnalités n’ont pas été activées pour votre compte, vos sélections concernant ces autorisations n’auront aucun effet.  

