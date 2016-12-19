---
author: mijacobs
Description: "Découvrez comment utiliser les vignettes, badges, toasts et notifications pour fournir des points d’entrée dans votre application et maintenir les utilisateurs informés."
title: Vignettes, badges et notifications
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: a02793e45f190b9401f18e845af3dc73d235c3fc

---
# Notifications de badge pour les applications UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

<div style="float:left; font-size:80%; text-align:left; margin: 0px 15px 15px 0px;">
<img src="images/badge-example.png" alt="A tile with a numeric badge displaying the number 63 to indicate 63 unread mails." style="padding-bottom:0.0em; margin-bottom: 2px" /><br/>Une vignette avec un badge numérique affichant<br/> le nombre 63 pour indiquer 63 e-mails non lus.</div>

Un badge de notification transmet des informations récapitulatives ou d’état propres à votre application. Ces informations peuvent être numériques (1 à 99) ou correspondre à l’un des ensembles de glyphes fournis par le système. Voici quelques exemples des informations les mieux transmises par l’intermédiaire d’un badge : état de la connexion réseau dans un jeu en ligne, statut de l’utilisateur dans une application de messagerie, nombre de messages non lus dans une application de courrier électronique et nombre de nouveaux billets dans une application de médias sociaux. 

Les badges de notification apparaissent sur l’icône de la barre des tâches de votre application et dans le coin inférieur droit de sa vignette d’accueil, que l’application soit en cours d’exécution ou non. Il est possible d’afficher les badges sur des vignettes de toute taille.  

**Remarque**  Il n’est pas possible de fournir votre propre image de badge. Seules les images de badge fournies par le système sont utilisables.

## Badges numériques

<table>
    <tr>
        <th>Valeur</th>
        <th>Badge</th>
        <th>XML</th>
    </tr>
    <tr>
        <td>Nombre compris entre 1 et 99 Une valeur de 0 est équivalente à la valeur de glyphe « aucuneˆ» et efface le badge.</td>
        <td>![Un badge numérique inférieur à 100.](images/badges/badge-numeric.png)</td>
        <td>`<badge value="1"/>`</td>
    </tr>
    <tr>
        <td>N’importe quel nombre supérieur à 99</td>
        <td>![Un badge numérique supérieur à 99.](images/badges/badge-numeric-greater.png)</td></td>
        <td>`<badge value="100"/>`</td>
    </tr>    
</table>

## Badges de glyphe
Au lieu d’un nombre, un badge peut afficher l’un des ensembles de glyphes d’état non extensibles. 

<table>
<tr>
    <th>État</th>
    <th>Glyphe</th>
    <th>XML</th>
</tr>
<tr>
    <td>aucune</td>
    <td>(Aucun badge affiché)</td>
    <td>`<badge value="none"/>`</td>
</tr>
<tr>
    <td>activité</td>
    <td>![Glyphe](images/badges/badge-activity.png)</td>
    <td>`<badge value="activity"/>`</td>
</tr>
<tr>
    <td>alarme</td>
    <td>![Glyphe](images/badges/badge-alarm.png)</td>
    <td>`<badge value="alarm"/>`</td>
</tr>
<tr>
    <td>alerte</td>
    <td>![Glyphe](images/badges/badge-alert.png)</td>
    <td>`<badge value="alert"/>`</td>
</tr>
<tr>
    <td>attention</td>
    <td>![Glyphe](images/badges/badge-attention.png)</td>
    <td>`<badge value="attention"/>`</td>
</tr>
<tr>
    <td>disponible</td>
    <td>![Glyphe](images/badges/badge-available.png)</td>
    <td>`<badge value="available"/>`</td>
</tr>
<tr>
    <td>absent</td>
    <td>![Glyphe](images/badges/badge-away.png)</td>
    <td>`<badge value="away"/>`</td>
</tr>
<tr>
    <td>occupé</td>
    <td>![Glyphe](images/badges/badge-busy.png)</td>
    <td>`<badge value="busy"/>`</td>
</tr>
<tr>
    <td>erreur</td>
    <td>![Glyphe](images/badges/badge-error.png)</td>
    <td>`<badge value="error"/>`</td>
</tr>
<tr>
    <td>nouveau message</td>
    <td>![Glyphe](images/badges/badge-newMessage.png)</td>
    <td>`<badge value="newMessage"/>`</td>
</tr>
<tr>
    <td>en pause</td>
    <td>![Glyphe](images/badges/badge-paused.png)</td>
    <td>`<badge value="paused"/>`</td>
</tr>
<tr>
    <td>lecture en cours</td>
    <td>![Glyphe](images/badges/badge-playing.png)</td>
    <td>`<badge value="playing"/>`</td>
</tr>
<tr>
    <td>non disponible</td>
    <td>![Glyphe](images/badges/badge-unavailable.png)</td>
    <td>`<badge value="unavailable"/>`</td>
</tr>
</table>

## Créer un badge

Ces exemples vous montrent comment créer une mise à jour de badge.

### Créer un badge numérique

````csharp
private void setBadgeNumber(int num)
{

    // Get the blank badge XML payload for a badge number
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeNumber);

    // Set the value of the badge in the XML to our number
    XmlElement badgeElement = badgeXml.SelectSingleNode("/badge") as XmlElement;
    badgeElement.SetAttribute("value", num.ToString());

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### Créer un badge de glyphe
````csharp
private void updateBadgeGlyph()
{
    string badgeGlyphValue = "alert";

    // Get the blank badge XML payload for a badge glyph
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeGlyph);

    // Set the value of the badge in the XML to our glyph value
    Windows.Data.Xml.Dom.XmlElement badgeElement = 
        badgeXml.SelectSingleNode("/badge") as Windows.Data.Xml.Dom.XmlElement;
    badgeElement.SetAttribute("value", badgeGlyphValue);

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### Effacer un badge

````csharp
private void clearBadge()
{
    BadgeUpdateManager.CreateBadgeUpdaterForApplication().Clear();
}
````

## Obtenir les exemples

* [Exemples de notification](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/Notifications)<br/> Montre comment créer des vignettes dynamiques, envoyer des mises à jour de badge et afficher des notifications toast. 

## Articles connexes

* [Notifications toast adaptatives et interactives](tiles-and-notifications-adaptive-interactive-toasts.md)
* [Créer des vignettes](tiles-and-notifications-creating-tiles.md)
* [Créer des vignettes adaptatives](tiles-and-notifications-create-adaptive-tiles.md)


<!--HONumber=Aug16_HO3-->


