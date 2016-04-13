---
title: Lancer l’application Contacts
description: Cette rubrique décrit le schéma d’URI ms-people. Votre application peut utiliser ce schéma d’URI afin de lancer l’application Contacts pour des actions spécifiques.
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
---

# Lancer l’application Contacts


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Cette rubrique décrit le schéma d’URI **ms-people:**. Votre application peut utiliser ce schéma d’URI afin de lancer l’application Contacts pour des actions spécifiques.

## Référence de schéma d’URI ms-people:


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Résultats</th>
<th align="left">Schéma d’URI</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Permet à d’autres applications de lancer la page Principaux de l’application Contacts.</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">Permet à d’autres applications de lancer la page Paramètres de l’application Contacts.</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">Permet à d’autres applications de fournir une chaîne de recherche qui lance l’application Contacts avec la page de résultats de la recherche.
<div class="alert">
**Remarque** <p>Les paramètres respectent la casse.</p>
<p>Si vous n’entrez pas la syntaxe correctement ou si la valeur de chaîne de recherche est manquante, le comportement par défaut consiste à renvoyer la liste complète des contacts sans aucun filtrage.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">Ouvre une carte de visite existante si le contact est trouvé. Ou bien ouvre une carte de visite temporaire si aucun contact n’est détecté. Si aucun paramètre n’est fourni, nous lançons l’application Contacts avec une liste de contacts.
<div class="alert">
**Remarque** <p>Les paramètres respectent la casse.</p>
<p>Peu importe l’ordre des paramètres.</p>
<p>S’il existe plusieurs correspondances, nous renvoyons la première correspondance du contact.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:viewcontact:?ContactId=&lt;contactid&gt;&amp;AggregatedId=&lt;aggid&gt;&amp;PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;&amp;Contact=&lt;contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">Ouvre une page d’enregistrement de contact dans l’application Contacts, permettant d’enregistrer le contact donné avec le numéro de téléphone ou l’adresse électronique fournis.
<div class="alert">
**Remarque** <p>Les paramètres respectent la casse.</p>
<p>Peu importe l’ordre des paramètres.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
</tbody>
</table>

 

## Référence de paramètre ms-people:search:


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Paramètre</th>
<th align="left">Description</th>
<th align="left">Exemple</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**SearchString**</td>
<td align="left"><p>Facultatif.</p>
<p>Chaîne de recherche pour les informations de recherche de contact.</p>
<p>Numéro de téléphone ou nom du contact.</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

 

## Référence de paramètre ms-people:viewcontact:


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Paramètre</th>
<th align="left">Description</th>
<th align="left">Exemple</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**ContactId**</td>
<td align="left"><p>Facultatif.</p>
<p>ID de contact du contact.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>Facultatif.</p>
<p>Numéro de téléphone du contact.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left">**Email**</td>
<td align="left"><p>Facultatif.</p>
<p>Adresse électronique du contact.</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left">**ContactName**</td>
<td align="left"><p>Facultatif.</p>
<p>Nom du contact.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left">**Contact**</td>
<td align="left"><p>Facultatif.</p>
<p>Objet Contact.</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

 

## Référence de paramètre ms-people:savetocontact:


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Paramètre</th>
<th align="left">Description</th>
<th align="left">Exemple</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>Facultatif.</p>
<p>Numéro de téléphone du contact.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left">**Email**</td>
<td align="left"><p>Facultatif.</p>
<p>Adresse électronique du contact.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left">**ContactName**</td>
<td align="left"><p>Facultatif.</p>
<p>Nom du contact.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

 

 

 





<!--HONumber=Mar16_HO1-->


