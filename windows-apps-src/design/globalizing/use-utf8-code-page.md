---
Description: Utiliser des caractères UTF-8 encodage pour la compatibilité optimale entre les applications web et autres * nix-plates-formes (Unix, Linux et variantes), de minimiser les bogues de localisation et de réduire le test de charge.
title: Utilisez la page de codes Windows UTF-8
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: a9386b31d16796c68d41a27ab48a5b2c9a9a342b
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141805"
---
# <a name="use-the-utf-8-code-page"></a>Utiliser la page de codes UTF-8

Utilisez [UTF-8](http://www.utf-8.com/) codage pour la compatibilité optimale entre les applications web et d’autres caractères * nix-plates-formes (Unix, Linux et variantes), de minimiser les bogues de localisation et de réduire le test de charge.

UTF-8 est la page de codes universelle pour l’internationalisation et prend en charge tous les points de code Unicode à l’aide de l’encodage de largeur variable 1 à 4 octets. Elle est utilisée de manière omniprésente sur le web et est la valeur par défaut pour * nix plates-formes.

## <a name="-a-vs--w-apis"></a>-A et API -W
  
API Win32 prennent souvent en charge les variantes - un et -W.

-A variantes reconnaissent la page de codes ANSI configurée sur le système et la prise en charge `char*`, tandis que les variantes -W fonctionnent en UTF-16 et la prise en charge `WCHAR`.

Jusqu'à récemment, Windows a mis en évidence les variantes -W « Unicode » sur - une API. Toutefois, les versions récentes ont utilisé la page de codes ANSI et - une API comme un moyen d’introduire UTF-8 prend en charge pour les applications. Si la page de codes ANSI est configurée pour UTF-8, - une API fonctionnent en UTF-8. Ce modèle présente l’avantage de prendre en charge le code existant intègrent - une API sans modification du code.

## <a name="set-a-process-code-page-to-utf-8"></a>Définir une page de codes de processus au format UTF-8

Depuis la Version 1903 (peut 2019 mettre à jour) de Windows, vous pouvez utiliser la propriété ActiveCodePage dans le fichier appxmanifest pour les applications packagées ou le manifeste de fusion pour les applications décompressées, pour forcer un processus à utiliser UTF-8 en tant que la page de codes de processus.

Vous pouvez déclarer cette propriété et cible/exécuter sur Windows antérieures les builds, mais vous devez gérer la détection des pages code hérité et conversion comme d’habitude. Avec une version cible minimale de Windows Version 1903, la page de codes du processus sera toujours UTF-8 afin de la conversion et la détection de page de code hérité peuvent être évités.

## <a name="examples"></a>Exemples

**Manifeste AppX pour une application empaquetée :**

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         ...
         xmlns:uap7="http://schemas.microsoft.com/appx/manifest/uap/windows10/7"
         xmlns:uap8="http://schemas.microsoft.com/appx/manifest/uap/windows10/8"
         ...
         IgnorableNamespaces="... uap7 uap8 ...">

  <Applications>
    <Application ...>
      <uap7:Properties>
        <uap8:ActiveCodePage>UTF-8</uap8:ActiveCodePage>
      </uap7:Properties>
    </Application>
  </Applications>
</Package>
```

**Manifeste de fusion pour une application Win32 décompressée :**

``` xaml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity type="win32" name="..." version="6.0.0.0"/>
  <application>
    <windowsSettings>
      <activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>
    </windowsSettings>
  </application>
</assembly>
```

> [!NOTE]
> Ajouter un manifeste pour un exécutable existant à partir de la ligne de commande avec `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>Conversion des pages de code

Comme Windows fonctionne en mode natif en UTF-16 (`WCHAR`), vous devrez peut-être convertir des données UTF-8 en UTF-16 (ou inversement) pour interagir avec les API Windows.

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) et [WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) permettent de vous effectuer une conversion entre UTF-8 et UTF-16 (`WCHAR`) (et d’autres pages de codes). Cela est particulièrement utile lorsqu’un héritage API Win32 peut comprendre uniquement `WCHAR`. Ces fonctions permettent de convertir l’entrée de UTF-8 au `WCHAR` pour passer à une API -W, puis convertir tous les résultats si nécessaire.
Lors de l’utilisation de ces fonctions avec `CodePage` définie sur `CP_UTF8`, utilisez `dwFlags` du `0` ou `MB_ERR_INVALID_CHARS`, sinon une `ERROR_INVALID_FLAGS` se produit.

Remarque : `CP_ACP` équivaut à `CP_UTF8` uniquement s’exécutant sur Windows Version 1903 (peut 2019 mettre à jour) ou une version ultérieure et la propriété ActiveCodePage décrit ci-dessus est définie sur UTF-8. Sinon, il respecte la page de codes système hérité. Nous vous recommandons d’utiliser `CP_UTF8` explicitement.

## <a name="related-topics"></a>Rubriques connexes

- [Pages de codes](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [Code Page Identifiers](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
