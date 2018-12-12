---
Description: When a resource is requested, there may be several candidates that match the current resource context to some degree. The Resource Management System will analyze all of the candidates and determine the best candidate to return. This topic describes that process in detail and gives examples.
title: Comment le système de gestion des ressources met en correspondance et sélectionne les ressources
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: de34411d9c7d226857214472e691dd6b41f10a18
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8943459"
---
# <a name="how-the-resource-management-system-matches-and-chooses-resources"></a>Comment le système de gestion des ressources met en correspondance et sélectionne les ressources
Lorsqu’une ressource est requise, plusieurs candidats sont susceptibles de correspondre, dans une certaine mesure, au contexte de ressource actuel. Le système de gestion des ressources analyse tous les candidats et identifie le meilleur à renvoyer. Pour ce faire, il prend en compte l’ensemble des qualificateurs afin de classer tous les candidats.

Dans ce processus de classement, une priorité différente est donnée aux qualificateurs. Ainsi, la langue a le plus d’impact sur le classement général et elle est suivie du contraste, puis de l’échelle et ainsi de suite. Pour chaque qualificateur, les qualificateurs candidats sont comparés à la valeur du qualificateur de contexte afin de déterminer la qualité de la correspondance. La façon dont la comparaison est effectuée varie en fonction du qualificateur.

Pour plus d’informations sur la mise en correspondance de balises de langue, consultez [Comment le système de gestion des ressources met en correspondance les balises de langue](how-rms-matches-lang-tags.md).

Pour certains qualificateurs, tels que l’échelle et le contraste, il existe toujours un degré minimal de correspondance. Par exemple, un candidat qualifié pour «scale-100 «correspond à un contexte de «scale-400» dans une certaine mesure petite, bien qu’elles pas ainsi que d’un candidat qualifié pour» scale-200» ou (pour une correspondance parfaite)» scale-400».

Toutefois, pour d’autres qualificateurs tels que la langue ou le lieu de résidence, il est possible d’avoir une comparaison sans correspondance (tout comme divers degrés de correspondance). Par exemple, un candidat dont le qualificateur de langue est «en-US» est une correspondance partielle pour un contexte «en-GB», mais un candidat dont le qualificateur est «fr» n’est pas du tout une correspondance. De même, un candidat dont le qualificateur de lieu de résidence est «155» (Europe de l’ouest) correspond plutôt bien au contexte d’un utilisateur dont le paramètre de lieu de résidence est «FR», mais un candidat ayant le qualificateur «US» ne correspond pas du tout.

Lors de l’évaluation d’un candidat, s’il existe une comparaison sans correspondance pour un qualificateur, ce candidat obtiendra un classement global de non correspondance et ne sera pas sélectionné. Ainsi, les qualificateurs ayant la priorité plus élevée peuvent avoir le plus d’impact dans la sélection de la meilleure correspondance, mais un qualificateur de faible priorité peut également éliminer un candidat en raison d’une non-correspondance.

Un candidat est neutre vis à vis d’un qualificateur s’il n’est pas marqué pour ce qualificateur. Pour tout qualificateur, un candidat neutre représente toujours une correspondance pour la valeur du qualificateur de contexte. Toutefois, sa qualité de correspondance est moins élevée que celle de tout candidat marqué pour ce qualificateur et ayant un degré de correspondance (exacte ou partielle). Par exemple, s’il existe des candidats qualifiés pour «en-US», «en» et «fr», ainsi qu’un candidat sans qualificateur de langue, pour un contexte dont la valeur de qualificateur de langue est «en-GB», les candidats sont classés dans l’ordre suivant: «en», «en-US», neutre et «fr». Dans ce cas, «fr» ne correspond pas à tout, tandis que les autres candidats présentent un certain degré de correspondance.

Le processus de classement global commence par l’évaluation des candidats par rapport au qualificateur ayant la priorité la plus élevée, c’est-à-dire la langue. Les non-correspondances sont éliminées. Les candidats restants sont classés en fonction de la qualité de correspondance pour la langue. Si des candidats sont à égalité, le qualificateur ayant la priorité la plus élevée suivante, c’est-à-dire le contraste, est pris en compte et la qualité de correspondance pour le contraste est donc utilisée pour différencier les candidats ex æquo. Après le contraste, le qualificateur d’échelle est utilisé pour séparer les candidats à égalité, et ainsi de suite, avec autant de qualificateurs que nécessaire pour parvenir au classement approprié.

Si tous les candidats sont écartés en raison de qualificateurs ne correspondant pas au contexte, le chargeur de ressources effectue une deuxième passe pour rechercher un candidat par défaut à afficher. Les candidats par défaut sont déterminés lors de la création du fichier PRI; ils sont nécessaires pour garantir la sélection d’un candidat pour n’importe quel contexte d’exécution. Si un candidat a des qualificateurs qui ne correspondent pas et ne sont pas des valeurs par défaut, cette ressource potentielle est définitivement éliminée.

Pour toutes les ressources potentielles encore prises en considération, le chargeur de ressources examine la valeur du qualificateur de contexte ayant la priorité la plus élevée et choisit celle présentant la meilleure correspondance ou le meilleur score par défaut. Une correspondance réelle est considérée meilleure qu’un score par défaut.

En cas d’égalité, la valeur du qualificateur de contexte ayant la priorité la plus élevée suivante est examinée et le processus se poursuit jusqu’à ce qu’une meilleure correspondance soit trouvée.

## <a name="example-of-choosing-a-resource-candidate"></a>Exemple de sélection d’une ressource potentielle
Prenons ces fichiers.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
fr/images/contrast-high/logo.scale-400.jpg
fr/images/contrast-high/logo.scale-100.jpg
de/images/logo.jpg
```

Et supposons qu’il s’agit des paramètres dans le contexte actuel.

```console
Application language: en-US; fr-FR;
Scale: 400
Contrast: Standard
```

Le système de gestion de ressources élimine trois des fichiers, car le contraste élevé et l’allemand ne correspondent pas au contexte défini par les paramètres. Il reste donc ces candidats.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

Pour les candidats restants, le système de gestion des ressources utilise le qualificateur de contexte avec la priorité la plus élevée, c’est-à-dire la langue. Les ressources anglaises sont une meilleure correspondance que les ressources françaises, car l’anglais est répertorié avant le français dans les paramètres.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
```

Ensuite, le système de gestion des ressources utilise le qualificateur de contexte ayant la priorité la plus élevée suivante, c’est à dire l’échelle. Voici la ressource renvoyée.

```console
en/images/logo.scale-400.jpg
```

Vous pouvez utiliser la méthode avancée [**NamedResource.ResolveAll**](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live) pour extraire tous les candidats dans l’ordre dans lequel ils correspondent aux paramètres du contexte. Pour l’exemple que nous venons de détailler, **ResolveAll** renvoie les candidats dans cet ordre.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

## <a name="example-of-producing-a-fallback-choice"></a>Exemple de production d’une sélection de secours
Prenons ces fichiers.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

Et supposons qu’il s’agit des paramètres dans le contexte actuel.

```console
User language: de-DE;
Scale: 400
Contrast: High
```

Tous les fichiers sont éliminés, car ils ne correspondent pas au contexte. Par conséquent, nous entrons une passe par défaut, où la valeur par défaut (voir [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)) était la suivante lors de la création du fichier PRI.

```console
Language: fr-FR;
Scale: 400
Contrast: Standard
```

Cela conserve toutes les ressources qui correspondent à l’utilisateur actuel ou à la valeur par défaut.

```console
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

Le système de gestion des ressources utilise le qualificateur de contexte ayant la priorité la plus élevée, c’est-à-dire la langue, pour renvoyer la ressource nommée avec le score le plus élevé.

```console
de/images/contrast-standard/logo.jpg
```

## <a name="important-apis"></a>API importantes
* [NamedResource.ResolveAll](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live)

## <a name="related-topics"></a>Rubriques associées
* [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)