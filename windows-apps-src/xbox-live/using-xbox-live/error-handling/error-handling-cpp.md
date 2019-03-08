---
title: Gestion des erreurs d'API C++
description: Découvrez comment gérer les erreurs lors de l’établissement d’un appel de service Xbox Live et les APIs C++.
ms.assetid: 10b47e68-8b1f-4023-96a4-404f3f6a9850
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, la gestion des erreurs
ms.localizationpriority: medium
ms.openlocfilehash: 90fd816f8d44b27c1df0ded9bee6473f642478a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636524"
---
# <a name="c-api-error-handling"></a>Gestion des erreurs d'API C++

Dans l’API C++, plutôt que de lever des exceptions, la plupart des appels retournera xbox_live_result < payload_type > comme il convient.

## <a name="xboxliveresult-structure"></a>structure de xbox_live_result
xbox_live_result a 3 éléments :
1. L’erreur retournée par l’opération,
2. Message d’erreur spécifique utilisée pour le débogage et
3. La charge utile du résultat (peut être vide si une erreur s’est produite) »

Vous pouvez obtenir plus d’informations sur xbox_live_result comme ainsi que l’erreur qui sont des codes dans la documentation de Xbox Live.

La structure est la suivante :

```cpp
template<typename T>
class xbox_live_result
{
    const std::error_code& err();
    const std::string& err_message();
    T& payload();
};
```

**Err** -renvoie l’erreur.  Sera une référence NULL sans erreur.  Il se comporte comme la bibliothèque STL C++ erreur le cas dans que vous pouvez obtenir la valeur primitive en appelant value().  Appel message(), vous obtiendrez une représentation de chaîne.  Par conséquent, si le code d’erreur signifie « Argument non valide », puis ```err().message()``` serait le texte « Argument non valide ».

**err_message** -s’articule autour de l’erreur.  Par exemple si **err** est « Argument non valide », puis **err_message** serait est élaboré sur quel argument n’est pas valide.

**charge utile** -retourner l’élément concerné.  Considérez par exemple ```xbox_live_result<achievement>``` que vous pouvez obtenir à partir de get_achievement appelant.  Dans cet exemple, la charge utile serait la prime elle-même (si aucune erreur n’est présent).

## <a name="example"></a>Exemple

```cpp
// Function which returns an xbox_live_result
xbox_live_result<std::shared_ptr<title_presence_change_subscription>> presenceChangeSubscriptionResult =
xbox::services::presence::subscribe_to_title_presence_change(
    xboxUserId,
    titleId
    );

printf("Error value %d, string %s", achievementResult.err().value(), achievementResult.err().message());

// Would output:
// "0 Success" if successful
// "401 Unauthorized" if auth issue

if (!achievementResult.err())
{
  // Do things on success.  Payload will be populated if applicable.
  std::shared_ptr<title_presence_change_subscription> presenceChangeSubscription = presenceChangeSubscriptionResult->payload();

  // ...
}
else if (achievement.err() == xboxlive_error_code::http_status_403_forbidden)
{
  // Special handling for 403 errors
}
else if (achievementResult.err() == xbox_live_error_condition::auth)
{
  // Handle broad auth failures.  See below section for more info on xbox_live_error_condition
}

```

## <a name="using-xboxliveerrorcondition-to-test-against-broad-error-categories"></a>À l’aide de xbox_live_error_condition à tester par rapport à des catégories d’erreur étendue
Dans l’exemple ci-dessus, nous testons le code d’erreur sur les 403 erreurs, ainsi que ce que l'on appelle ```xbox_live_error_condition::auth```.

 Lorsque vous utilisez xbox_live_result (fonction) Err (), un peut tester individuellement contre les codes d’erreur.  Par exemple, pour les erreurs de type 400, vous pourriez avoir individuelles de test et de contrôle de flux pour :

* xbox_live_error_code::http_status_400_bad_request
* xbox_live_error_code::http_status_401_unauthorized
* xbox_live_error_code::http_status_403_forbidden
* etc

Mais en général, il s’agit pas ce que vous voulez faire et à tester par rapport à une classe d’erreurs en tant qu’un.  Pour que vous puissiez tester par rapport à une classe d’erreurs à l’aide des énumérations disponibles dans le ```xbox_live_error_condition``` classe.  Nous implémentons une surcharge pour l’opérateur d’égalité qui automatise les test par rapport à nombreux codes d’erreur.  En plus de ```auth```, il existe des catégories, telles que ```rta``` et ```http```.  Vous trouverez la liste complète dans *errors.h* ou dans *xblsdk_cpp.chm*.

Pour une vidéo qui traite cela et autres fonctionnalités de l’API de Service Xbox C++, consultez notre discussion XFest dans [Xfest 2015 vidéos](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) sous *XSAPI : C++, aucune exception !*
