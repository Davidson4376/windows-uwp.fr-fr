---
author: Xansky
description: How to use contacts and calendar info in your UWP app.
title: Contacts and calendar
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
translationtype: Human Translation
ms.sourcegitcommit: 5c0f6ef1f1a346a66ca554a415d9f24c8a314ae1
ms.openlocfilehash: da73790ca9aec3fa16295eac4880c7b80db033ab

---

# Contacts and calendar

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

You can let your users access their contacts and appointments so they can share content, email, calendar info, or messages with each other, or whatever functionality you design.

To see a few different ways in which your app can access contacts and appointments, see these topics:

| Topic | Description |
|-------|-------------|
| [Select contacts](selecting-contacts.md) | Through the [<strong>Windows.ApplicationModel.Contacts</strong>](https://msdn.microsoft.com/library/windows/apps/BR225002) namespace, you have several options for selecting contacts. Here, we'll show you how to select a single contact or multiple contacts, and we'll show you how to configure the contact picker to retrieve only the contact information that your app needs. |
| [Send email](sending-email.md) | Shows how to launch the compose email dialog to allow the user to send an email message. You can pre-populate the fields of the email with data before showing the dialog. The message will not be sent until the user taps the send button. |
| [Send an SMS message](sending-an-sms-message.md) | This topic shows you how to launch the compose SMS dialog to allow the user to send an SMS message. You can pre-populate the fields of the SMS with data before showing the dialog. The message will not be sent until the user taps the send button. |
| [Manage appointments](managing-appointments.md) | Through the [<strong>Windows.ApplicationModel.Appointments</strong>](https://msdn.microsoft.com/library/windows/apps/Dn263359) namespace, you can create and manage appointments in a user's calendar app. Here, we'll show you how to create an appointment, add it to a calendar app, replace it in the calendar app, and remove it from the calendar app. We'll also show how to display a time span for a calendar app and create an appointment-recurrence object. |
| [Connect your app to actions on a contact card](integrating-with-contacts.md) | Shows how to make your app appear next to actions on a contact card or mini contact card. Users can choose your app to perform an action such as open a profile page, place a call, or send a message. |
| [Provide social feeds to the People app](integrating-social-feeds-into-contact-cards.md) | Integrate social feed data from your database into the People app. Your feed data will appear in the <strong>What's New</strong> pages of the People app or in the <strong>Profile</strong> page of a contact. |

 

## Related topics

* [Appointments API sample](http://go.microsoft.com/fwlink/p/?linkid=309836)
* [Contact manager API sample](http://go.microsoft.com/fwlink/p/?LinkID=310079)
* [Contact Picker app sample](http://go.microsoft.com/fwlink/p/?linkid=231575)
* [Handling Contact Actions sample](http://go.microsoft.com/fwlink/p/?LinkID=320151)
* [Contact Card Integration Sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)
* [Social Info Sample](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp)



<!--HONumber=Aug16_HO5-->


