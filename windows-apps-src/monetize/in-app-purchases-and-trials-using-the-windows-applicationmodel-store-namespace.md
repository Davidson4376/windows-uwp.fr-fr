---
author: Xansky
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: Découvrez comment activer les achats in-app et les versions d’évaluation dans les applications UWP qui ciblent les versions antérieures à Windows10 version1607.
title: Versions d’évaluation et achats dans l’application à l’aide de l’espace de noms Windows.ApplicationModel.Store
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
keywords: uwp, achats dans l’application, extensions, versions d’évaluation, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: 330631afed95c3b0082de69d9369a62aad5a66d5
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5945846"
---
# <a name="in-app-purchases-and-trials-using-the-windowsapplicationmodelstore-namespace"></a>Versions d’évaluation et achats dans l’application à l’aide de l’espace de noms Windows.ApplicationModel.Store

Vous pouvez utiliser les membres de l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) pour ajouter des achats in-app, et la fonctionnalité d’évaluation dans votre application de plateforme Windows universelle (UWP) pour monétiser votre application. Ces API offrent également l’accès aux informations de licence de votre application.

Les articles de cette section fournissent des instructions détaillées et des exemples de code pour utiliser les membres de l’espace de noms **Windows.ApplicationModel.Store** dans plusieurs scénarios courants. Pour une vue d’ensemble des concepts liés aux achats in-app dans les applications UWP, consultez [Achats in-app et versions d’évaluation](in-app-purchases-and-trials.md). Pour obtenir un exemple complet montrant comment implémenter des versions d’évaluation et des achats dans l'application à l’aide de l’espace de noms **Windows.ApplicationModel.Store**, consultez l’[Exemple WindowsStore](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store).

> [!IMPORTANT]
> L'espace de noms **Windows.ApplicationModel.Store** n'est plus mis à jour avec de nouvelles fonctionnalités. Si votre projet d’app cible **Windows 10 Anniversary Edition (version10.0; Build14393)** ou une version ultérieure dans Visual Studio (ce qui correspond à Windows10, version1607 ou versions ultérieures), nous vous recommandons d’utiliser l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) à la place. Pour plus d’informations, consultez [Versions d’évaluation et achats dans l'application](https://msdn.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials). L'espace de noms **Windows.ApplicationModel.Store** n’est pas pris en charge dans les applications de bureau Windows qui utilisent le [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop) ou dans les apps ou jeux qui utilisent un développement en bac à sable dans le centre de développement (par exemple, dans le cas des jeux qui s’intègrent à Xbox Live). Ces produits doivent utiliser l’espace de noms **Windows.Services.Store** pour implémenter les achats in-app et les versions d’évaluation.

## <a name="get-started-with-the-currentapp-and-currentappsimulator-classes"></a>Prise en main des classes CurrentApp et CurrentAppSimulator

Le point d’entrée principal de l’espace de noms **Windows.ApplicationModel.Store** est la classe [CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx). Cette classe fournit des propriétés et des méthodes statiques que vous pouvez utiliser, entre autres, pour obtenir des informations sur l’application active et ses extensions disponibles, acheter une application ou une extension pour l’utilisateur actuel, et obtenir des informations sur la licence de l’application en cours ou de ses extensions.

La classe [CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) obtient ses données à partir du MicrosoftStore. Vous devez donc disposer d’un compte de développeur et l’app doit être publiée dans le Store pour que vous puissiez utiliser cette classe dans votre app. Avant de soumettre votre app au Store, vous pouvez tester votre code avec une version de cette classe appelée [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx). Après avoir testé votre app et avant de la soumettre au MicrosoftStore, vous devez remplacer les instances de **CurrentAppSimulator** par **CurrentApp**. Votre application ne sera pas certifiée si elle utilise **CurrentAppSimulator**.

Lorsque **CurrentAppSimulator** est utilisé, l’état initial des produits in-app et de licence de votre application est décrit dans un fichier local nommé WindowsStoreProxy.xml, situé sur votre ordinateur de développement. Pour plus d’informations sur ce fichier, consultez [Utilisation du fichier WindowsStoreProxy.xml avec CurrentAppSimulator](#proxy).

Pour plus d’informations sur les tâches courantes exécutables avec **CurrentApp** et **CurrentAppSimulator**, consultez les articles suivants.

| Rubrique       | Description                 |
|----------------------------|-----------------------------|
| [Exclure ou limiter des fonctionnalités de la version d’évaluation](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | Si vous donnez aux clients la possibilité d’utiliser votre application gratuitement pendant une période d’évaluation, vous pouvez leur donner envie de mettre à niveau vers la version complète de votre application en excluant ou en limitant certaines fonctionnalités pendant la période d’évaluation. |
| [Activer les achats de produits in-app](enable-in-app-product-purchases.md)      |  Que votre application soit gratuite ou non, vous pouvez vendre du contenu, d’autres applications ou de nouvelles fonctionnalités applicatives (par exemple le déverrouillage d’un nouveau niveau de jeu) directement dans l’application. Nous allons vous montrer comment activer ces produits dans votre application.  |
| [Activer les achats de produits consommables in-app](enable-consumable-in-app-product-purchases.md)      | Proposez des produits consommables dans l’application qui peuvent être achetés, utilisés et rachetés via la plateforme commerciale du Windows Store, afin d’offrir à vos clients une expérience d’achat à la fois solide et fiable au sein de l’application. Cette fonction est particulièrement utile pour différents aspects du jeu, comme les devises (or, pièces, etc.) susceptibles d’être achetées, puis utilisées pour acheter certaines améliorations. |
| [Gérer un vaste catalogue de produits in-app](manage-a-large-catalog-of-in-app-products.md)      |   Si votre application propose un vaste catalogue de produits in-app, vous pouvez éventuellement suivre la procédure décrite dans cette rubrique pour faciliter la gestion de votre catalogue.    |
| [Utiliser des reçus pour vérifier les achats de produits](use-receipts-to-verify-product-purchases.md)      |   Chaque transaction du MicrosoftStore qui entraîne un achat de produit peut éventuellement retourner un reçu de transaction qui fournit des informations sur le produit répertorié et le coût monétaire pour le client. L’accès à ces informations autorise les scénarios dans lesquels votre app doit vérifier qu’un utilisateur a acheté votre app ou qu’il a effectué des achats in-app de produits dans le MicrosoftStore. |

<span id="proxy" />

## <a name="using-the-windowsstoreproxyxml-file-with-currentappsimulator"></a>Utilisation du fichier WindowsStoreProxy.xml avec CurrentAppSimulator

Lorsque **CurrentAppSimulator** est utilisé, l’état initial des produits in-app et de licence de votre application est décrit dans un fichier local nommé WindowsStoreProxy.xml, situé sur votre ordinateur de développement. Les méthodes **CurrentAppSimulator** qui modifient l’état de l’application, par exemple en achetant une licence ou en gérant un achat in-app, mettent simplement à jour l’état de l’objet **CurrentAppSimulator** en mémoire. Le contenu du fichier WindowsStoreProxy.xml n’est pas modifié. Au redémarrage de l’application, la licence reprend l’état décrit dans le fichier WindowsStoreProxy.xml.

Un fichier WindowsStoreProxy.xml est créé par défaut à l’emplacement suivant: %UserProfile%\AppData\Local\Packages\\&lt;dossier du package d’application&gt;\LocalState\Microsoft\Windows Store\ApiData. Vous pouvez modifier ce fichier pour définir le scénario que vous voulez simuler dans les propriétés de **CurrentAppSimulator**.

Si vous pouvez modifier les valeurs dans ce fichier, nous vous recommandons de créer votre propre fichier WindowsStoreProxy.xml (dans un dossier de données de votre projet Visual Studio) pour **CurrentAppSimulator** et de l’utiliser à la place de l’autre. Lors de la simulation de la transaction, appelez [ReloadSimulatorAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.reloadsimulatorasync) pour charger votre fichier. Si vous n’appelez pas **ReloadSimulatorAsync** pour charger votre propre fichier WindowsStoreProxy.xml, **CurrentAppSimulator** crée/charge (mais ne remplace pas) le fichier WindowsStoreProxy.xml par défaut.

> [!NOTE]
> N’oubliez pas que **CurrentAppSimulator** ne s’initialise pleinement qu’une fois **ReloadSimulatorAsync** exécuté. Et, dans la mesure où **ReloadSimulatorAsync** est une méthode asynchrone, veillez à éviter la condition de concurrence avec l’interrogation de **CurrentAppSimulator** sur un thread pendant son initialisation sur un autre. Une technique consiste à utiliser un indicateur pour signaler la fin de l’initialisation. Une app installée à partir du MicrosoftStore doit utiliser **CurrentApp** à la place de **CurrentAppSimulator**. Dans ce cas, **ReloadSimulatorAsync** n’est pas appelé et la condition de concurrence mentionnée auparavant ne s’applique pas. Pour cette raison, concevez votre code afin qu’il fonctionne dans les deuxcas de figure (asychrone et synchrone).


<span id="proxy-examples" />

### <a name="examples"></a>Exemples

Cet exemple est un fichier WindowsStoreProxy.xml (codé en UTF-16) qui décrit une application dont le mode évaluation arrive à expiration le 19janvier 2015 à 05:00 (UTC).

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="UTF-16"?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>2B14D306-D8F8-4066-A45B-0FB3464C67F2</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/2B14D306-D8F8-4066-A45B-0FB3464C67F2</LinkUri>
      <CurrentMarket>en-US</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with a trial license</Name>
        <Description>Sample app for demonstrating trial license management</Description>
        <Price>4.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>true</IsTrial>
      <ExpirationDate>2015-01-19T05:00:00.00Z</ExpirationDate>
    </App>
  </LicenseInformation>
  <Simulation SimulationMode="Automatic">
    <DefaultResponse MethodName="LoadListingInformationAsync_GetResult" HResult="E_FAIL"/>
  </Simulation>
</CurrentApp>
```

L’exemple suivant est un fichier WindowsStoreProxy.xml (codé en UTF-16) qui décrit une application achetée, dotée d’une fonctionnalité expirant le 19janvier2015 à 05:00 (UTC), et associée à un achat in-app consommable.

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-16" ?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>988b90e4-5d4d-4dea-99d0-e423e414ffbc</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/988b90e4-5d4d-4dea-99d0-e423e414ffbc</LinkUri>
      <CurrentMarket>en-us</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with several in-app products</Name>
        <Description>Sample app for demonstrating an expiring in-app product and a consumable in-app product</Description>
        <Price>5.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
    <Product ProductId="feature1" LicenseDuration="10" ProductType="Durable">
      <MarketData xml:lang="en-us">
        <Name>Expiring Item</Name>
        <Price>1.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
    <Product ProductId="consumable1" LicenseDuration="0" ProductType="Consumable">
      <MarketData xml:lang="en-us">
        <Name>Consumable Item</Name>
        <Price>2.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>false</IsTrial>
    </App>
    <Product ProductId="feature1">
      <IsActive>true</IsActive>
      <ExpirationDate>2015-01-19T00:00:00.00Z</ExpirationDate>
    </Product>
  </LicenseInformation>
  <ConsumableInformation>
    <Product ProductId="consumable1" TransactionId="00000001-0000-0000-0000-000000000000" Status="Active"/>
  </ConsumableInformation>
</CurrentApp>
```


<span id="proxy-schema" />

### <a name="schema"></a>Schéma

Cette section présente le fichierXSD qui définit la structure du fichier WindowsStoreProxy.xml. Pour appliquer ce schéma à l’éditeur XML dans Visual Studio lorsque vous utilisez votre fichier WindowsStoreProxy.xml, procédez comme suit:

1. Ouvrez le fichier WindowsStoreProxy.xml dans Visual Studio.
2. Dans le menu **XML**, cliquez sur **Créer un schéma**. Le fichier WindowsStoreProxy.xsd temporaire est créé en fonction du contenu du fichierXML.
3. Remplacez le contenu de ce fichierXSD par le schéma ci-dessous.
4. Enregistrez le fichier là où vous pouvez l’appliquer à plusieurs projets d’application.
5. Basculez vers le fichier WindowsStoreProxy.xml dans Visual Studio.
6. Dans le menu **XML**, cliquez sur **Schémas**, puis recherchez la ligne correspondant au fichier WindowsStoreProxy.xsd dans la liste. Si l’emplacement du fichier n’est pas celui que vous souhaitez (par exemple, si le fichier temporaire s’affiche toujours), cliquez sur **Ajouter**. Accédez au fichier, puis cliquez sur **OK**. Vous devez maintenant voir ce fichier dans la liste. Vérifiez qu’une coche apparaît dans la colonne **Utilisation** de ce schéma.

Une fois ceci fait, les modifications apportées à WindowsStoreProxy.xml sont soumises au schéma. Pour plus d’informations, consultez [Procédure: sélectionner les schémas XML à utiliser](http://go.microsoft.com/fwlink/p/?LinkId=403014).

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:import namespace="http://www.w3.org/XML/1998/namespace"/>
  <xs:element name="CurrentApp" type="CurrentAppDefinition"></xs:element>
  <xs:complexType name="CurrentAppDefinition">
    <xs:sequence>
      <xs:element name="ListingInformation" type="ListingDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LicenseInformation" type="LicenseDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ConsumableInformation" type="ConsumableDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Simulation" type="SimulationDefinition" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:simpleType name="ResponseCodes">
    <xs:restriction base="xs:string">
      <xs:enumeration value="S_OK">
        <xs:annotation>
          <xs:documentation>0x00000000</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_INVALIDARG">
        <xs:annotation>
          <xs:documentation>0x80070057</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_CANCELLED">
        <xs:annotation>
          <xs:documentation>0x800704C7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_FAIL">
        <xs:annotation>
          <xs:documentation>0x80004005</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_OUTOFMEMORY">
        <xs:annotation>
          <xs:documentation>0x8007000E</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="ERROR_ALREADY_EXISTS">
        <xs:annotation>
          <xs:documentation>0x800700B7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="ConsumableStatus">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Active"/>
      <xs:enumeration value="PurchaseReverted"/>
      <xs:enumeration value="PurchasePending"/>
      <xs:enumeration value="ServerError"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="StoreMethodName">
    <xs:restriction base="xs:string">
      <xs:enumeration value="RequestAppPurchaseAsync_GetResult" id="RPPA"/>
      <xs:enumeration value="RequestProductPurchaseAsync_GetResult" id="RFPA"/>
      <xs:enumeration value="LoadListingInformationAsync_GetResult" id="LLIA"/>
      <xs:enumeration value="ReportConsumableFulfillmentAsync_GetResult" id="RPFA"/>
      <xs:enumeration value="LoadListingInformationByKeywordsAsync_GetResult" id="LLIKA"/>
      <xs:enumeration value="LoadListingInformationByProductIdAsync_GetResult" id="LLIPA"/>
      <xs:enumeration value="GetUnfulfilledConsumablesAsync_GetResult" id="GUC"/>
      <xs:enumeration value="GetAppReceiptAsync_GetResult" id="GARA"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="SimulationMode">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Interactive"/>
      <xs:enumeration value="Automatic"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ListingDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppListingDefinition"/>
      <xs:element name="Product" type="ProductListingDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ConsumableDefinition">
    <xs:sequence>
      <xs:element name="Product" type="ConsumableProductDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppListingDefinition">
    <xs:sequence>
      <xs:element name="AppId" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LinkUri" type="xs:anyURI" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrentMarket" type="xs:language" minOccurs="1" maxOccurs="1"/>
      <xs:element name="AgeRating" type="xs:unsignedInt" minOccurs="1" maxOccurs="1"/>
      <xs:element name="MarketData" type="MarketSpecificAppData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="MarketSpecificAppData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="MarketSpecificProductData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Tag" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Keywords" type="KeywordDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="ImageUri" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="ProductListingDefinition">
    <xs:sequence>
      <xs:element name="MarketData" type="MarketSpecificProductData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="LicenseDuration" type="xs:integer" use="optional"/>
    <xs:attribute name="ProductType" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:simpleType name="guid">
    <xs:restriction base="xs:string">
      <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ConsumableProductDefinition">
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="TransactionId" type="guid" use="required"/>
    <xs:attribute name="Status" type="ConsumableStatus" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="LicenseDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppLicenseDefinition"/>
      <xs:element name="Product" type="ProductLicenseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="IsTrial" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ProductLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="ProductId" type="xs:string" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="SimulationDefinition" >
    <xs:sequence>
      <xs:element name="DefaultResponse" type="DefaultResponseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="SimulationMode" type="SimulationMode" use="optional"/>
  </xs:complexType>
  <xs:complexType name="DefaultResponseDefinition">
    <xs:attribute name="MethodName" type="StoreMethodName" use="required"/>
    <xs:attribute name="HResult" type="ResponseCodes" use="required"/>
  </xs:complexType>
  <xs:complexType name="KeywordDefinition">
    <xs:sequence>
      <xs:element name="Keyword" type="xs:string" minOccurs="0" maxOccurs="10"/>
    </xs:sequence>
  </xs:complexType>
</xs:schema>
```


<span id="proxy-descriptions" />

### <a name="element-and-attribute-descriptions"></a>Description des éléments et des attributs

Cette section décrit les éléments et attributs dans le fichier WindowsStoreProxy.xml.

L’élément racine de ce fichier est l’élément **CurrentApp** qui représente l’application active. Cet élément contient les éléments enfants suivants:

|  Élément  |  Requis  |  Quantité  |  Description   |
|-------------|------------|--------|--------|
|  [ListingInformation](#listinginformation)  |    Oui        |  1  |  Contient les données de la liste de l’application.            |
|  [LicenseInformation](#licenseinformation)  |     Oui       |   1    |   Décrit les licences disponibles pour cette application et ses modules complémentaires durables.     |
|  [ConsumableInformation](#consumableinformation)  |      Non      |   0 ou 1   |   Décrit les modules complémentaires consommables disponibles pour cette application.      |
|  [Simulation](#simulation)  |     Non       |      0 ou 1      |   Décrit le fonctionnement des appels à plusieurs méthodes [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) dans l’application lors du test.    |

<span id="listinginformation" />

#### <a name="listinginformation-element"></a>Élément ListingInformation

Cet élément contient les données de la liste de l’application. **ListingInformation** est un enfant requis de l’élément **CurrentApp**.

**ListingInformation** contient les éléments enfants suivants.

|  Élément  |  Requis  |  Quantité  |  Description   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-listinginformation)  |    Oui   |  1   |    Fournit des données sur l’application.         |
|  [Product](#product-child-of-listinginformation)  |    Non  |  0 ou davantage   |      Décrit un module complémentaire de l’application.     |     |

<span id="app-child-of-listinginformation"/>

#### <a name="app-element-child-of-listinginformation"></a>Élément App (enfant de ListingInformation)

Cet élément décrit la licence de l’application. **App** est un enfant requis de l’élément [ListingInformation](#listinginformation).

**App** contient les éléments enfants suivants.

|  Élément  |  Requis  |  Quantité  | Description   |
|-------------|------------|--------|--------|
|  **AppId**  |    Oui   |  1   |   GUID identifiant l’application dans le WindowsStore. Cela peut être le GUID utilisé pour le test.        |
|  **LinkUri**  |    Oui  |  1   |    URI de la page de liste dans le Windowsstore. Cela peut être n’importe quel URL valide pour le test.         |
|  **CurrentMarket**  |    Oui  |  1   |    Pays/région du client.         |
|  **AgeRating**  |    Oui  |  1   |     Entier représentant la classification d’âge minimum de l’application. Il s’agit de la même valeur que vous spécifiez dans le tableau de bord du Centre de développement lorsque vous soumettez l’application. Les valeurs utilisées par le WindowsStore sont: 3, 7, 12 et 16. Pour plus d’informations sur ces classifications, consultez [Classification par âge](../publish/age-ratings.md).        |
|  [MarketData](#marketdata-child-of-app)  |    Oui  |  1 ou davantage      |    Contient des informations sur l’application pour un pays/une région donné(e). Pour chaque pays/région où l’application est répertoriée, vous devez inclure un élément **MarketData**.       |    |

<span id="marketdata-child-of-app"/>

#### <a name="marketdata-element-child-of-app"></a>Élément MarketData (enfant d’App)

Cet élément fournit des informations sur l’application pour un pays/une région donné(e). Pour chaque pays/région où l’application est répertoriée, vous devez inclure un élément **MarketData**. **MarketData** est un enfant requis de l’élément [App](#app-child-of-listinginformation).

**MarketData** contient les éléments enfants suivants.

|  Élément  |  Requis  |  Quantité  | Description   |
|-------------|------------|--------|--------|
|  **Name**  |    Oui   |  1   |   Nom de l’application dans ce pays/cette région.        |
|  **Description**  |    Oui  |  1   |      Description de l’application dans ce pays/cette région.       |
|  **Price**  |    Oui  |  1   |     Prix de l’application dans ce pays/cette région.        |
|  **CurrencySymbol**  |    Oui  |  1   |     Symbole de devise utilisé dans ce pays/cette région.        |
|  **CurrencyCode**  |    Non  |  0 ou 1      |      Code de devise utilisé dans ce pays/cette région.         |  |

**MarketData** a les attributs suivants.

|  Attribut  |  Requis  |  Description   |
|-------------|------------|----------------|
|  **xml:lang**  |    Oui        |     Spécifie le pays/la région où les données de marché s’appliquent.          |  |

<span id="product-child-of-listinginformation"/>

#### <a name="product-element-child-of-listinginformation"></a>Élément Product (enfant de ListingInformation)

Cet élément décrit un module complémentaire de l’application. **Product** est un enfant facultatif de l’élément [ListingInformation](#listinginformation) et il contient un ou plusieurs éléments [MarketData](#marketdata-child-of-product)s.

**Product** a les attributs suivants.

|  Attribut  |  Requis  |  Description   |
|-------------|------------|----------------|
|  **ProductId**  |    Oui        |    Contient la chaîne utilisée par l’application pour identifier le module complémentaire.           |
|  **LicenseDuration**  |    Non        |    Indique le nombre de jours pendant lesquels la licence reste valide, une fois l’élément acheté. La date d’expiration de la nouvelle licence créée par un achat de produit correspond à la date d’achat avec la durée de la licence. Cet attribut n’est utilisé que si l’attribut **ProductType** a pour valeur **Durable**. Il est ignoré pour les modules complémentaires consommables.           |
|  **ProductType**  |    Non        |    Contient une valeur permettant d’identifier la persistance du produit in-app. Les valeurs prises en charge sont **Durable** (valeur par défaut) et **Consumable**. Pour les types durables, les informations supplémentaires sont décrites par un élément [Product](#product-child-of-licenseinformation) sous [LicenseInformation](#licenseinformation). Pour les types consommables, les informations supplémentaires sont décrites par un élément [Product](#product-child-of-consumableinformation) sous [ConsumableInformation](#consumableinformation).           |  |

<span id="marketdata-child-of-product"/>

#### <a name="marketdata-element-child-of-product"></a>Élément MarketData (enfant de Product)

Cet élément fournit des informations sur le module complémentaire pour un pays/une région donné(e). Pour chaque pays/région où le module complémentaire est répertorié, vous devez inclure un élément **MarketData**. **MarketData** est un enfant requis de l’élément [Product](#product-child-of-listinginformation).

**MarketData** contient les éléments enfants suivants.

|  Élément  |  Requis  |  Quantité  | Description   |
|-------------|------------|--------|--------|
|  **Name**  |    Oui   |  1   |   Nom du module complémentaire dans ce pays/cette région.        |
|  **Price**  |    Oui  |  1   |     Prix du module complémentaire dans ce pays/cette région.        |
|  **CurrencySymbol**  |    Oui  |  1   |     Symbole de devise utilisé dans ce pays/cette région.        |
|  **CurrencyCode**  |    Non  |  0 ou 1      |      Code de devise utilisé dans ce pays/cette région.         |  
|  **Description**  |    Non  |   0 ou 1   |      Description du module complémentaire pour ce pays/cette région.       |
|  **Tag**  |    Non  |   0 ou 1   |      [Données personnalisées du développeur](../publish/enter-add-on-properties.md#custom-developer-data) (également appelées balise) du module complémentaire.       |
|  **Keywords**  |    Non  |   0 ou 1   |      Contient jusqu’à 10éléments **Keyword** qui contiennent les [mots clés](../publish/enter-add-on-properties.md#keywords) du module complémentaire.       |
|  **ImageUri**  |    Non  |   0 ou 1   |      [URI de l’image](../publish/create-add-on-store-listings.md#icon) dans la liste du module complémentaire.           |  |

**MarketData** a les attributs suivants.

|  Attribut  |  Requis  |  Description   |
|-------------|------------|----------------|
|  **xml:lang**  |    Oui        |     Spécifie le pays/la région où les données de marché s’appliquent.          |  |

<span id="licenseinformation"/>

#### <a name="licenseinformation-element"></a>Élément LicenseInformation

Cet élément décrit les licences disponibles pour cette application et ses produits in-app durables. **LicenseInformation** est un enfant requis de l’élément **CurrentApp**.

**LicenseInformation** contient les éléments enfants suivants.

|  Élément  |  Requis  |  Quantité  | Description   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-licenseinformation)  |    Oui   |  1   |    Décrit la licence de l’application.         |
|  [Product](#product-child-of-licenseinformation)  |    Non  |  0 ou davantage   |      Décrit l’état de la licence d’un module complémentaire durable dans l’application.         |   |

Le tableau suivant montre comment simuler certaines conditions courantes en combinant les valeurs des éléments **App** et **Product**.

|  Condition à simuler  |  IsActive  |  IsTrial  | ExpirationDate   |
|-------------|------------|--------|--------|
|  Licence complète  |    true   |  false  |    Absent. Cet élément peut être présent et spécifier une date future, mais il est recommandé de l’omettre du fichier XML. S’il est présent et spécifie une date passée, **IsActive** est ignoré et considéré comme ayant la valeur false.          |
|  In trial period  |    true  |  true   |      &lt;horodatage futur&gt; Cet élément doit être présent car **IsTrial** a la valeur true. Vous pouvez visiter un siteWeb affichant l’heure UTC (Temps universel coordonné) pour savoir quelle date choisir et obtenir la période d’évaluation restante souhaitée.         |
|  Expired trial  |    false  |  true   |      &lt;horodatage passé&gt; Cet élément doit être présent car **IsTrial** a la valeur true. Vous pouvez visiter un siteWeb affichant l’heure UTC (Temps Universel Coordonné) pour savoir depuis combien de temps l’heureUTC est passée.         |
|  Invalid  |    false  | false       |     &lt;n’importe quelle valeur ou valeur omise&gt;          |  |

<span id="app-child-of-licenseinformation"/>

#### <a name="app-element-child-of-licenseinformation"></a>Élément App (enfant de LicenseInformation)

Cet élément décrit la licence de l’application. **App** est un enfant requis de l’élément [LicenseInformation](#licenseinformation).

**App** contient les éléments enfants suivants.

|  Élément  |  Requis  |  Quantité  | Description   |
|-------------|------------|--------|--------|
|  **IsActive**  |    Oui   |  1   |    Décrit l’état actuel de la licence de cette application. La valeur **true** indique que la licence est valide. La valeur **false** indique une licence non valide. Normalement, cette valeur est **true**, que l’application ait un mode d’évaluation ou non.  Réglez cette valeur sur **false** pour tester le comportement de votre application quand sa licence n’est pas valide.           |
|  **IsTrial**  |    Oui  |  1   |      Décrit l’état actuel d’évaluation de cette application. La valeur **true** indique que l’application est utilisée pendant la période d’évaluation. La valeur **false** indique que l’application n’est pas en période d’évaluation, soit parce qu’elle a été achetée, soit parce que la période d’évaluation est échue.         |
|  **ExpirationDate**  |    Non  |  0 ou 1       |     Date à laquelle la période d’évaluation de cette application expire, en temps universel coordonné (UTC). La date doit se présenter comme suit: aaaa-mm-jjThh:mm:ss.ssZ. Par exemple, le 19janvier2015 à05:00 correspond à 2015-01-19T05:00:00.00Z. Cet élément est requis lorsque **IsTrial** est **true**. Sinon, il est facultatif.          |  |

<span id="product-child-of-licenseinformation"/>

#### <a name="product-element-child-of-licenseinformation"></a>Élément Product (enfant de LicenseInformation)

Cet élément décrit l’état de la licence d’un module complémentaire durable dans l’application. **Product** est un enfant facultatif de l’élément [LicenseInformation](#licenseinformation).

**Product** contient les éléments enfants suivants.

|  Élément  |  Requis  |  Quantité  | Description   |
|-------------|------------|--------|--------|
|  **IsActive**  |    Oui   |  1     |    Décrit l’état actuel de la licence de ce module complémentaire. La valeur **true** indique que le module complémentaire est utilisable. La valeur **false** indique que le module complémentaire n’est pas utilisable ou n’a pas été acheté.           |
|  **ExpirationDate**  |    Non   |  0 ou 1     |     Date d’expiration du module complémentaire, en temps universel coordonné (UTC). La date doit se présenter comme suit: aaaa-mm-jjThh:mm:ss.ssZ. Par exemple, le 19janvier2015 à05:00 correspond à 2015-01-19T05:00:00.00Z. Si cet élément est présent, le module complémentaire a une date d’expiration. S’il n’est pas présent, le module complémentaire n’expire pas.  |  

**Product** a les attributs suivants.

|  Attribut  |  Requis  |  Description   |
|-------------|------------|----------------|
|  **ProductId**  |    Oui        |   Contient la chaîne utilisée par l’application pour identifier le module complémentaire.            |
|  **OfferId**  |     Non       |   Contient la chaîne utilisée par l’application pour identifier la catégorie à laquelle appartient le module complémentaire. Il permet de prendre en charge des catalogues volumineux, comme indiqué dans [Gérer un vaste catalogue de produits intégrés à l'application](manage-a-large-catalog-of-in-app-products.md).           |

<span id="simulation"/>

#### <a name="simulation-element"></a>Élément de simulation

Cet élément décrit le fonctionnement des appels à plusieurs méthodes [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) dans l’application lors du test. **Simulation** est un enfant facultatif de l’élément **CurrentApp** et contient zéro, un ou plusieurs éléments [DefaultResponse](#defaultresponse).

**Simulation** a les attributs suivants.

|  Attribut  |  Requis  |  Description   |
|-------------|------------|----------------|
|  **SimulationMode**  |    Non        |      La valeur peut être **Interactive** ou **Automatic**. Lorsque cet attribut a pour valeur **Automatic**, les méthodes renvoient automatiquement les codes d’erreur HRESULT spécifiés. Il peut s’utiliser lors de l’exécution de scénarios de test automatisés.       |

<span id="defaultresponse"/>

#### <a name="defaultresponse-element"></a>Élément DefaultResponse

Cet élément décrit le code d’erreur par défaut renvoyé par une méthode **CurrentAppSimulator**. **DefaultResponse** est un enfant facultatif de l’élément [Simulation](#simulation).

**DefaultResponse** a les attributs suivants.

|  Attribut  |  Requis  |  Description   |
|-------------|------------|----------------|
|  **MethodName**  |    Oui        |   Affectez à cet attribut l’une des valeurs d’énumération affichées pour le type **StoreMethodName** dans le [schéma](#schema). Chacune de ces valeurs d’énumération représente une méthode **CurrentAppSimulator** pour laquelle vous voulez simuler la valeur de retour du code d’erreur dans votre application au cours du test. Par exemple, la valeur **RequestAppPurchaseAsync_GetResult** indique que vous voulez simuler la valeur de retour du code d’erreur de la méthode [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.requestapppurchaseasync).            |
|  **HResult**  |     Oui       |   Affectez à cet attribut l’une des valeurs d’énumération affichées pour le type **ResponseCodes** dans le [schéma](#schema). Chacune de ces valeurs d’énumération représente le code d’erreur que vous voulez renvoyer pour la méthode affectée à l’attribut **MethodName** de cet élément **DefaultResponse**.           |

<span id="consumableinformation"/>

#### <a name="consumableinformation-element"></a>Élément ConsumableInformation

Cet élément décrit les modules complémentaires consommables disponibles pour cette application. **ConsumableInformation** est un enfant facultatif de l’élément **CurrentApp** et contient zéro, un ou plusieurs éléments [Product](#product-child-of-consumableinformation).

<span id="product-child-of-consumableinformation"/>

#### <a name="product-element-child-of-consumableinformation"></a>Élément Product (enfant de ConsumableInformation)

Cet élément décrit un module complémentaire consommable. **Product** est un enfant facultatif de l’élément [ConsumableInformation](#consumableinformation).

**Product** a les attributs suivants.

|  Attribut  |  Requis  |  Description   |
|-------------|------------|----------------|
|  **ProductId**  |    Oui        |   Contient la chaîne utilisée par l’application pour identifier le module complémentaire consommable.            |
|  **TransactionId**  |     Oui       |   Contient un GUID (sous forme de chaîne) utilisé pour suivre la transaction d’achat d’un consommable via le processus d’acquisition. Consultez [Activer l’achat de produits in-app consommables](enable-consumable-in-app-product-purchases.md).            |
|  **Status**  |      Oui      |  Contient la chaîne utilisée par l’application pour indiquer l’état d’acquisition d’un consommable. La valeur peut être **Active**, **PurchaseReverted**, **PurchasePending** ou **ServerError**.             |
|  **OfferId**  |     Non       |    Contient la chaîne utilisée par l’application pour identifier la catégorie à laquelle appartient le consommable. Il permet de prendre en charge des catalogues volumineux, comme indiqué dans [Gérer un vaste catalogue de produits intégrés à l'application](manage-a-large-catalog-of-in-app-products.md).           |
