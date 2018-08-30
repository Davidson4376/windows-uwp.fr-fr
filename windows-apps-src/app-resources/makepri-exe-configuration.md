---
author: stevewhims
Description: This topic describes the schema of the MakePri.exe XML configuration file.
title: Fichier de configuration de MakePri.exe
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 512880b7a7ea955a45697762cbbdb7f74ac70102
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3127354"
---
# <a name="makepriexe-configuration-file"></a>Fichier de configuration de MakePri.exe

Cette rubrique décrit le schéma du fichier de configuration XML de [MakePri.exe](compile-resources-manually-with-makepri.md), également appelé fichier de configuration PRI. L’outil MakePri.exe inclut une [commande createconfig](makepri-exe-command-options.md#createconfig-command) qui vous permet de créer un nouveau fichier de configuration PRI initialisé.

> [!NOTE]
> MakePri.exe est installé lorsque vous vérifiez l’option **Du SDK Windows pour les applications UWP managées** lors de l’installation du Kit de développement logiciel Windows. Il est installé sur le chemin d’accès `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (ainsi que dans les dossiers nommés pour les autres architectures). Exemple: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

Le ficher de configuration PRI détermine les ressources qui sont indexées et la façon dont l’indexation est effectuée. Le fichier de configuration XML doit être conforme au schéma suivant.

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="resources">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="packaging" maxOccurs="1" minOccurs="0">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="autoResourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:attribute name="qualifier" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
              <xs:element name="resourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="qualifierSet" maxOccurs="unbounded" minOccurs="0">
                      <xs:complexType>
                        <xs:attribute name="definition" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                  <xs:attribute name="name" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element maxOccurs="unbounded" name="index">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="qualifiers" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="default" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="indexer-config" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip"/>
                  </xs:sequence>
                  <xs:attribute name="type" type="xs:string" use="required" />
                  <xs:anyAttribute processContents="skip"/>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
            <xs:attribute name="root" type="xs:string" use="required" />
            <xs:attribute name="startIndexAt" type="xs:string" use="required" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
      <xs:attribute name="isDeploymentMergeable" type="xs:boolean" use="optional" />
      <xs:attribute name="majorVersion" type="xs:positiveInteger" use="optional" />
      <xs:attribute name="targetOsVersion" type="xs:string" use="optional" />
    </xs:complexType>
  </xs:element>
</xs:schema>
```

- L’élément `default` spécifie le contexte (langue, échelle, contraste, etc.) qui doit être utilisé pour résoudre les ressources lorsque le contexte d’exécution ne correspond à aucune ressource potentielle. Dans la mesure où ce contexte est spécifié au moment de la génération et ne change pas, les ressources sont résolues en ce contexte lorsque les qualificateurs sont créés. Le score correspondant est enregistré lors de la génération. Chaque qualificateur doit avoir une valeur spécifiée. Voir [ResourceContext](resource-management-system.md#resourcecontext) pour plus d’informations sur la façon dont les ressources sont choisies.
- L’élément `index` définit les passes d’indexation discrètes qui sont effectuées sur les ressources. Chaque passe d’indexation détermine les [indexeurs spécifiques au format](makepri-exe-format-specific-indexers.md) à utiliser et les ressources à indexer.
- L’élément `qualifiers` définit les qualificateurs initiaux pour le premier fichier ou dossier dont héritent d’autres ressources. Chaque élément «qualifier» doit avoir un nom et une valeur valides (voir [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)).
- L’attribut `root` est la racine du chemin d’accès du fichier physique pour la passe d’index. Il peut être absolu ou relatif. S’il est relatif, il est ajouté à la racine du projet que vous fournissez dans la ligne de commande. S’il est absolu, il est utilisé directement en tant que racine de passe d’index. Les barres obliques ou barres obliques inversées sont acceptées. Les barres obliques finales sont tronquées. La racine de la passe d’index détermine le dossier dans lequel toutes les ressources sont considérées comme relatives.
- L’attribut `startIndexAt` est le fichier ou dossier seed initial utilisé pour l’indexation. Il est relatif à la racine de passe d’index. Une valeur vide suppose qu’il s’agit du dossier racine de passe d’index.

## <a name="default-pri-config-file"></a>Fichier de configuration par défaut PRI

MakePri.exe génère ce nouveau fichier de configuration PRI initialisé lors de l’utilisation de la [commande createconfig](makepri-exe-command-options.md#createconfig-command).

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<resources targetOsVersion="10.0.0" majorVersion="1">
  <packaging>
    <autoResourcePackage qualifier="Language"/>
    <autoResourcePackage qualifier="Scale"/>
    <autoResourcePackage qualifier="DXFeatureLevel"/>
  </packaging>
  <index root="\" startIndexAt="\">
    <default>
      <qualifier name="Language" value="en-US"/>
      <qualifier name="Contrast" value="standard"/>
      <qualifier name="Scale" value="100"/>
      <qualifier name="HomeRegion" value="001"/>
      <qualifier name="TargetSize" value="256"/>
      <qualifier name="LayoutDirection" value="LTR"/>
      <qualifier name="Theme" value="dark"/>
      <qualifier name="AlternateForm" value=""/>
      <qualifier name="DXFeatureLevel" value="DX9"/>
      <qualifier name="Configuration" value=""/>
      <qualifier name="DeviceFamily" value="Universal"/>
      <qualifier name="Custom" value=""/>
    </default>
    <indexer-config type="folder" foldernameAsQualifier="true" filenameAsQualifier="true" qualifierDelimiter="."/>
    <indexer-config type="resw" convertDotsToSlashes="true" initialPath=""/>
    <indexer-config type="resjson" initialPath=""/>
    <indexer-config type="PRI"/>
  </index>
  <!--<index startIndexAt="Start Index Here" root="Root Here">-->
  <!--        <indexer-config type="resfiles" qualifierDelimiter="."/>-->
  <!--        <indexer-config type="priinfo" emitStrings="true" emitPaths="true" emitEmbeddedData="true"/>-->
  <!--</index>-->
</resources>
```

## <a name="packaging-element"></a>Élément packaging

L’élément `packaging` définit les informations de division PRI. Le schéma de l’élément `packaging` est défini pour la configuration automatique (prise en charge de `autoResourcePackage` avec une dimension spécifique) et la configuration manuelle.

Cet exemple montre comment utiliser `autoResourcePackage` avec une dimension spécifique.

```xml
    <packaging>
        <autoResourcePackage qualifier="Language"/>
        <autoResourcePackage qualifier="Scale"/>
        <autoResourcePackage qualifier="DXFeatureLevel"/>
    </packaging>
```

Cet exemple montre comment configurer `resourcePackage` manuellement.

```xml
  <packaging>
    <resourcePackage name="Germany">
      <qualifierSet definition="lang-de-de"/>
      <qualifierSet definition="lang-es-es"/>
    </resourcePackage>  
    <resourcePackage name="France">
      <qualifierSet definition="lang-fr-fr"/>
    </resourcePackage>  
    <resourcePackage name="HighRes1">
      <qualifierSet definition="scale-200"/>
    </resourcePackage>
    <resourcePackage name="HighRes2">
      <qualifierSet definition="scale-400"/>
    </resourcePackage>
  </packaging>
```

MakePri.exe ne bloque pas de manière explicite la génération de fichiers PRI de ressources pour une dimension spécifique. Des restrictions applicables à un certain ensemble de dimensions sont définies et implémentées en externe par MakeAppx.exe ou d’autres outils dans le pipeline.

MakePri.exe analyse l’élément `packaging` après tous les nœuds `index` pour remplir tous les qualificateurs par défaut. MakePri.exe collecte les informations obtenues dans ces structures de données.

```csharp
enum ResourcePackageMode
{
    None,
    AutoPackQualifier,
    ManualPack
}

ResourcePackageMode eResourcePackageMode;
list<string> RPQualifierList; // To store AutoResourcePackage Qualifiers
map<string, list<string>> RPNameToQSIMap; // To store ResourcePackage name to QualifierSet list mapping.
```

## <a name="resourcesisdeploymentmergeable-attribute"></a>Attribut resources@isDeploymentMergeable

Cet attribut définit un indicateur dans le fichier PRI qui entraîne les actions suivantes

- Fusion de déploiement pour identifier que ce fichier PRI peut fusionner.
- GetFullyQualifiedReference renvoie une erreur si cet indicateur est défini et que le gestionnaire de ressources a été initialisé avec un fichier.

La valeur par défaut de cet attribut est `true`. MakePri.exe définit l’indicateur dans PRI uniquement si vous ciblez Windows10.

Nous vous conseillons d’omettre `isDeploymentMergeable` (ou de définir cet attribut de manière explicite sur `true`) pour la création d’un pack de ressources si vous ciblez Windows10.

MakePri.exe ajoute la valeur de `isDeploymentMergeable` dans le fichier de vidage si `makepri dump` est exécuté avec l’option `/dt detailed`.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <IsDeploymentMergeable>true</IsDeploymentMergeable>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="resourcesmajorversion-attribute"></a>Attribut resources@majorVersion

La valeur par défaut de cet attribut est1. Si vous fournissez une valeur explicite, et que vous utilisez également l’option de ligne de commande `/VersionMajor(vma)` qui est déconseillée pour l’outil MakePri.exe, la valeur figurant dans le fichier de configuration est prioritaire.

Voici un exemple.

```xml
<resources majorVersion="2">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

## <a name="resourcestargetosversion-attribute"></a>Attribut resources@targetOsVersion

Indique la version cible du système d’exploitation. Le tableau ci-dessous indique les valeurs qui sont prises en charge, la valeur par défaut étant 6.3.0.

| Valeur | Signification |
| ----- | ------- |
| 10.0.0 | Windows10 |
| 6.3.0 (par défaut) | Windows8.1 |
| 6.2.1 | Windows8 |

Voici un exemple.

```xml
<resources targetOsVersion="10.0.0">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

**Remarque:** Windows offre une compatibilité descendante pour les fichiers PRI, mais n’est pas toujours compatible avec la version installée.

MakePri.exe ajoute la valeur de `targetOsVersion` dans le fichier de vidage si `makepri dump` est exécuté avec l’option `/dt detailed`.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <TargetOS version="10.0.0"/>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="validation-error-messages"></a>Messages d’erreur lors de la validation

Voici quelques exemples de condition d’erreur et le message d’erreur correspondant.

| Condition | Gravité | Message |
| --------- | -------- | ------- |
| Une valeur targetOsVersion autre que celles prises en charge est spécifiée. | Erreur | Configuration non valide: valeur targetOsVersion non valide spécifiée. |
| Une valeur «6.2.1» est spécifiée pour targetOsVersion et un élément `packaging` est présent. | Erreur | Configuration non valide: le nœud «Packaging» n’est pas pris en charge avec cette valeur targetOsVersion. |
| Plusieurs modes ont été trouvés dans la configuration. Par exemple, Manual et AutoResourcePackage sont spécifiés. | Erreur | Configuration non valide: le nœud «packaging» ne peut pas avoir plus d’un mode de fonctionnement. |
| Un qualificateur par défaut est répertorié sous le package de ressources. | Erreur | Configuration non valide: <Qualifiername>=<QualifierValue> est un qualificateur par défaut et ses candidats ne peuvent pas être ajoutés à un package de ressources. |
| Le qualificateur AutoResourcePackage contient plusieurs qualificateurs. Par exemple, language_scale. | Erreur | Configuration non valide: AutoResourcePackage avec plusieurs qualificateurs n’est pas pris en charge. |
| ResourcePackage QualifierSet contient plusieurs qualificateurs. Par exemple, language-en-us_scale-100 | Erreur | Configuration non valide: QualifierSet avec plusieurs qualificateurs n’est pas pris en charge. |
| Nom de pack de ressources en double trouvé. | Erreur | Configuration non valide: nom de pack de ressources <rpname> dupliqué. |
| Le même qualificateur a été défini dans deux packages de ressources. | Erreur | Configuration non valide: plusieurs instances de QualifierSet «<qualifier tags>» trouvées. |
| Aucune candidat n’a été trouvé pour le QualifierSet répertorié pour le nœud «ResourcePackage». | Avertissement | Configuration non valide: aucun candidat trouvé pour <Resource Package Name>. |
| Aucun candidat n’a été trouvé pour le qualificateur répertorié sous le nœud «AutoResourcePackage». | Avertissement | Configuration non valide: aucun candidat trouvé pour le qualificateur <qualifier name>. Package de ressources non généré. |
| Aucun mode trouvé. Autrement dit, nœud «packaging» vide. | Avertissement | Configuration non valide: aucun mode de mise en package spécifié. |

## <a name="related-topics"></a>Rubriques associées

* [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)
* [Options de ligne de commande de MakePri.exe - commande createconfig](makepri-exe-command-options.md#createconfig-command)
* [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)
* [Système de gestion des ressources - ResourceContext](resource-management-system.md#resourcecontext)