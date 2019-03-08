---
Description: Cette rubrique décrit les indexeurs spécifiques au format utilisés par l’outil MakePri.exe pour générer son index de ressources.
title: Indexeurs spécifiques au format de MakePri.exe
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 1a245c4ec0280f687cf34e85123960e64fe36a57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645874"
---
# <a name="makepriexe-format-specific-indexers"></a>Indexeurs spécifiques au format de MakePri.exe

Cette rubrique décrit les indexeurs spécifiques au format utilisés par l’outil [MakePri.exe](compile-resources-manually-with-makepri.md) pour générer son index de ressources.

> [!NOTE]
> MakePri.exe est installé lorsque vous activez le **Kit de développement logiciel Windows pour les applications UWP managées** option lors de l’installation du Kit de développement logiciel Windows. Il est installé dans le chemin d’accès `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (ainsi que dans les dossiers nommés pour les autres architectures). Exemple : `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

MakePri.exe est généralement utilisé avec les commandes `new`, `versioned` ou `resourcepack`. Voir [Options de ligne de commande de MakePri.exe](makepri-exe-command-options.md). Dans ces cas, il indexe les fichiers sources pour générer un index des ressources. MakePri.exe utilise plusieurs indexeurs individuels pour lire les fichiers de ressources sources ou les conteneurs pour les ressources. L’indexeur de base est l’indexeur de dossier. Il indexe le contenu d’un dossier, par exemple des images `.jpg` ou `.png`.

Vous identifiez des indexeurs spécifiques au format en spécifiant les éléments `<indexer-config>` dans un élément `<index>` du [fichier de configuration de MakePri.exe](makepri-exe-configuration.md). L’attribut `type` identifie l’indexeur spécifique au format qui est utilisé.

En général, le contenu des conteneurs de ressources trouvés lors de l’indexation est indexé, au lieu d’être lui-même ajouté à l’index. Par exemple, les fichiers `.resjson` détectés par l’indexeur de dossier peuvent être eux-mêmes indexés par un indexeur `.resjson`, auquel cas le fichier `.resjson` n’apparaît pas dans l’index. **Remarque :** un élément `<indexer-config>` pour l’indexeur associé à ce conteneur doit être inclus dans le fichier de configuration pour que cela se produise.

En règle générale, les qualificateurs trouvés dans une entité contenante, par exemple un dossier ou un fichier`.resw`, sont appliqués à toutes les ressources figurant dans celle-ci, par exemple les fichiers du dossier ou les chaînes du fichier `.resw`.

## <a name="folder"></a>Dossier

L’indexeur de dossier est identifié par un attribut `type` FOLDER. Il indexe le contenu d’un dossier et détermine les qualificateurs de ressources à partir du dossier et des noms de fichiers. Son élément de configuration doit être conforme au schéma suivant.

```xml
<xs:schema attributeFormDefault=\"unqualified\" elementFormDefault=\"qualified\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">\
    <xs:simpleType name=\"ExclusionTypeList\">\
        <xs:restriction base=\"xs:string\">\
            <xs:enumeration value=\"path\"/>\
            <xs:enumeration value=\"extension\"/>\
            <xs:enumeration value=\"name\"/>\
            <xs:enumeration value=\"tree\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:complexType name=\"FolderExclusionType\">\
        <xs:attribute name=\"type\" type=\"ExclusionTypeList\" use=\"required\"/>\
        <xs:attribute name=\"value\" type=\"xs:string\" use=\"required\"/>\
        <xs:attribute name=\"doNotTraverse\" type=\"xs:boolean\" use=\"required\"/>\
        <xs:attribute name=\"doNotIndex\" type=\"xs:boolean\" use=\"required\"/>\
    </xs:complexType>\
    <xs:simpleType name=\"IndexerConfigFolderType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:sequence>\
                <xs:element name=\"exclude\" type=\"FolderExclusionType\" minOccurs=\"0\" maxOccurs=\"unbounded\"/>\
            </xs:sequence>\
            <xs:attribute name=\"type\" type=\"IndexerConfigFolderType\" use=\"required\"/>\
            <xs:attribute name=\"foldernameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"filenameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>
```

L’attribut `qualifierDelimiter` spécifie le caractère après lequel les qualificateurs sont spécifiés dans un nom de fichier, en ignorant l’extension. La valeur par défaut est « . ».

## <a name="pri"></a>PRI

L’indexeur PRI est identifié par un attribut `type` PRI. Il indexe le contenu d’un fichier PRI. En général, vous l’utilisez lors de l’indexation de la ressource contenue dans un autre assembly, DLL, kit de développement logiciel (SDK) ou bibliothèque de classes dans le fichier PRI de l’application.

Tous les noms de ressources, qualificateurs et valeurs contenus dans le fichier PRI sont gérés directement dans le nouveau fichier PRI. Toutefois, le mappage de ressources de niveau supérieur n’est pas géré dans le fichier PRI final. Les mappages de ressources sont fusionnés.

```xml
<xs:schema id=\"prifile\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigPriType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((p|P)(r|R)(i|I))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigPriType\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

## <a name="priinfo"></a>PriInfo

L’indexeur PriInfo est identifié par un attribut `type` PRIINFO. Il indexe le contenu d’un fichier de vidage détaillé. Vous produisez un fichier de vidage détaillé en exécutant `makepri dump` avec l’option `/dt detailed`. L’élément de configuration de l’indexeur doit être conforme au schéma suivant.

```xml
<xs:schema id="priinfo" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
  <xs:simpleType name="IndexerConfigPriInfoType">
    <xs:restriction base="xs:string">
      <xs:pattern value="((p|P)(r|R)(i|I)(i|I)(n|N)(f|F)(o|O))"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:element name="indexer-config">
    <xs:complexType>
      <xs:attribute name="type" type="IndexerConfigPriInfoType" use="required"/>
      <xs:attribute name="emitStrings" type="xs:boolean" use="optional"/>
      <xs:attribute name="emitPaths" type="xs:boolean" use="optional"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

Cet élément de configuration permet d’utiliser des attributs facultatifs pour configurer le comportement de l’indexeur PriInfo. La valeur par défaut est `emitStrings` et la valeur de `emitPaths` est `true`. Si la valeur de `emitStrings` est `true`, les ressources potentielles dont l’attribut `type` est défini sur « String » doivent être incluses dans l’index, sinon elles sont exclues. Si la valeur de « emitPaths » est `true`, les ressources potentielles dont l’attribut `type` est défini sur « Path » doivent être incluses dans l’index, sinon elles sont exclues.

Voici un exemple de configuration qui inclut les types de ressources String, mais ignore les types de ressources Path.

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

Pour être indexé, un fichier de vidage doit se terminer par l’extension `.pri.xml`et doit être conforme au schéma suivant.

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >\
  <xs:simpleType name="candidateType">\
    <xs:restriction base="xs:string">\
      <xs:pattern value="Path|String"/>\
    </xs:restriction>\
  </xs:simpleType>\
  <xs:complexType name="scopeType">\
    <xs:sequence>\
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>\
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">\
        <xs:complexType>\
          <xs:sequence>\
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:anyAttribute processContents="skip" />\
              </xs:complexType>\
            </xs:element>\
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">\
                    <xs:complexType>\
                      <xs:sequence>\
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">\
                          <xs:complexType>\
                            <xs:attribute name="name" type="xs:string" use="required" />\
                            <xs:attribute name="value" type="xs:string" use="required" />\
                            <xs:attribute name="priority" type="xs:integer" use="required" />\
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />\
                            <xs:attribute name="index" type="xs:integer" use="required" />\
                          </xs:complexType>\
                        </xs:element>\
                      </xs:sequence>\
                      <xs:anyAttribute processContents="skip" />\
                    </xs:complexType>\
                  </xs:element>\
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:attribute name="type" type="candidateType" use="required" />\
              </xs:complexType>\
            </xs:element>\
          </xs:sequence>\
          <xs:attribute name="name" use="required" type="xs:string" />\
          <xs:anyAttribute processContents="skip" />\
        </xs:complexType>\
      </xs:element>\
    </xs:sequence>\
    <xs:attribute name="name" use="required" type="xs:string" />\
    <xs:anyAttribute processContents="skip" />\
  </xs:complexType>\
  <xs:element name="PriInfo">\
    <xs:complexType>\
      <xs:sequence>\
        <xs:element name="PriHeader" >\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="QualifierInfo">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="ResourceMap">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:element name="VersionInfo">\
                <xs:complexType>\
                  <xs:anyAttribute processContents="skip" />\
                </xs:complexType>\
              </xs:element>\
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />\
            </xs:sequence>\
            <xs:attribute name="name" type="xs:string" use="required" />\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
      </xs:sequence>\
    </xs:complexType>\
  </xs:element>\
</xs:schema>
```

MakePri.exe prend en charge les types de vidage « Basic », « Detailed », « Schema » et « Summary ». Pour configurer MakePri.exe afin qu’il utilise un type de vidage que l’indexeur PriInfo est en mesure de lire, incluez « /DumpType Detailed » lorsque vous utilisez la commande `dump`.

Plusieurs éléments du fichier `.pri.xml` sont ignorés par MakePri.exe. Ces éléments sont calculés lors de l’indexation, ou spécifiés dans le fichier de configuration de MakePri.exe. Les noms de ressources, les qualificateurs et les valeurs contenus dans le fichier de vidage sont gérés directement dans le nouveau fichier PRI. Toutefois, le mappage de ressources de niveau supérieur n’est pas géré dans le fichier PRI final. Les mappages de ressources sont fusionnés dans le cadre de l’indexation.

Voici un exemple de ressource de type String valide à partir d’un fichier de vidage.

```xml
<NamedResource name="SampleString " index="96" uri="ms-resource://SampleApp/resources/SampleString ">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
  </Decision>
  <Candidate type="String">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>A Sample String Value</Value>
  </Candidate>
</NamedResource>
```

Voici un exemple de ressource de type Path valide avec deux candidats à partir d’un fichier de vidage.

```xml
<NamedResource name="Sample.png" index="77" uri="ms-resource://SampleApp/Files/Images/Sample.png">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="0.7" index="2"/>
    </QualifierSet>
  </Decision>
  <Candidate type="Path">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-180.png</Value>
  </Candidate>
  <Candidate type="Path">
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-140.png</Value>
  </Candidate>
</NamedResource>
```

## <a name="resfiles"></a>ResFiles

L’indexeur ResFiles est identifié par un attribut `type` RESFILES. Il indexe le contenu d’un fichier `.resfiles`. Son élément de configuration doit être conforme au schéma suivant.

```xml
<xs:schema id=\"resx\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResFilesType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResFilesType\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

Un fichier `.resfiles` est un fichier texte contenant une liste plate de chemins d’accès de fichiers. Un fichier `.resfiles` peut contenir des commentaires « // ». Voici un exemple :

```
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
```

## <a name="resjson"></a>ResJSON

L’indexeur ResJSON est identifié par un attribut `type` RESJSON. Il indexe le contenu d’un fichier `.resjson`, qui est un fichier de ressources de chaîne. Son élément de configuration doit être conforme au schéma suivant.

```xml
<xs:schema id=\"resjson\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResJsonType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResJsonType\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

Un fichier `.resjson` contient du texte JSON (voir [The application/json Media Type for JavaScript Object Notation (JSON)](https://www.ietf.org/rfc/rfc4627.txt)). Le fichier doit contenir un objet JSON unique avec des propriétés hiérarchiques. Chaque propriété doit être un autre objet JSON ou une valeur de chaîne.

Les propriétés JSON dont les noms commencent par un trait de soulignement (« _ ») ne sont pas compilées dans le fichier PRI final, mais sont gérées dans le fichier journal.

Le fichier peut également contenir des commentaires « // » qui sont ignorés lors de l’analyse.

L’attribut `initialPath` place toutes les ressources sous ce chemin d’accès initial en l’ajoutant au début du nom de la ressource. En général, vous l’utilisez lors de l’indexation de ressources de bibliothèques de classes. La valeur par défaut est vide.

## <a name="resw"></a>ResW

L’indexeur ResW est identifié par un attribut `type` RESW. Il indexe le contenu d’un fichier `.resw`, qui est un fichier de ressources de chaîne. Son élément de configuration doit être conforme au schéma suivant.

```xml
<xs:schema id=\"resw\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResxType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(w|W))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResxType\" use=\"required\"/>\
            <xs:attribute name=\"convertDotsToSlashes\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

Un fichier `.resw` est un fichier XML qui se conforme au schéma suivant.

```xml
  <xsd:schema id="root" xmlns="" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">
    <xsd:import namespace="http://www.w3.org/XML/1998/namespace" />
    <xsd:element name="root" msdata:IsDataSet="true">
      <xsd:complexType>
        <xsd:choice maxOccurs="unbounded">
          <xsd:element name="metadata">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" />
              </xsd:sequence>
              <xsd:attribute name="name" use="required" type="xsd:string" />
              <xsd:attribute name="type" type="xsd:string" />
              <xsd:attribute name="mimetype" type="xsd:string" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="assembly">
            <xsd:complexType>
              <xsd:attribute name="alias" type="xsd:string" />
              <xsd:attribute name="name" type="xsd:string" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="data">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
                <xsd:element name="comment" type="xsd:string" minOccurs="0" msdata:Ordinal="2" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" msdata:Ordinal="1" />
              <xsd:attribute name="type" type="xsd:string" msdata:Ordinal="3" />
              <xsd:attribute name="mimetype" type="xsd:string" msdata:Ordinal="4" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="resheader">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" />
            </xsd:complexType>
          </xsd:element>
        </xsd:choice>
      </xsd:complexType>
    </xsd:element>
  </xsd:schema>
```

L’attribut `convertDotsToSlashes` convertit tous les points (« . ») trouvés dans les noms de ressources (attributs de nom d’élément de données) en barre oblique « / », sauf lorsque les points se trouvent entre les caractères « [ » et « ] ».

L’attribut `initialPath` place toutes les ressources sous ce chemin d’accès initial en l’ajoutant au début du nom de la ressource. En général, vous l’utilisez lors de l’indexation de ressources de bibliothèques de classes. La valeur par défaut est vide.

## <a name="related-topics"></a>Rubriques connexes

* [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)
* [Options de ligne de commande MakePri.exe](makepri-exe-command-options.md)
* [Fichier de configuration MakePri.exe](makepri-exe-configuration.md)
* [Type de média application/json pour JavaScript Objet Notation (JSON)](https://www.ietf.org/rfc/rfc4627.txt)
