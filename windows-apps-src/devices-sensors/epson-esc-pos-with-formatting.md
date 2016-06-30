---
author: DBirtolo
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: Epson ESC/POS avec mise en forme
description: "Découvrez comment utiliser le langage de commandes ESC/POS pour mettre en forme le texte, par exemple pour le mettre en gras ou pour doubler la taille des caractères, pour l’imprimante de votre point de service."
translationtype: Human Translation
ms.sourcegitcommit: ba620bc89265cbe8756947e1531759103c3cafef
ms.openlocfilehash: 33231abb1e573d837193daeee21713f09c7d8448

---
# Epson ESC/POS avec mise en forme

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

** API importantes **

-   [Imprimante PointofService](https://msdn.microsoft.com/library/windows/apps/Mt426652)
-   [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

Découvrez comment utiliser le langage de commandes ESC/POS pour mettre en forme le texte, par exemple pour le mettre en gras ou pour doubler la taille des caractères, pour l’imprimante de votre Point de service.

## Utilisation des commandes ESC/POS

Le point de service Windows prend en charge l’utilisation d’une large gamme d’imprimantes, notamment plusieurs imprimantes de la gamme EPSON TM (pour consulter la liste complète des imprimantes prises en charge, consultez la page [Imprimante PointofService](https://msdn.microsoft.com/library/windows/apps/Mt426652)). Windows prend en charge l’impression via le langage de contrôle d’imprimante ESC/POS, qui fournit des commandes efficaces et fonctionnelles permettant de communiquer avec votre imprimante.

ESC/POS est un système de commande créé par Epson qui est utilisé sur une gamme élargie de systèmes d’imprimantes POS. Il a vocation à éliminer l’incompatibilité des ensembles de commandes, en assurant une applicabilité universelle. La plupart des imprimantes modernes prennent en charge ESC/POS.

L’ensemble des commandes commencent par les caractères ESC (ASCII 27, HEX 1B) ou GS (ASCII 29, HEX 1D), suivis par un autre caractère spécifiant la commande. Les éléments textuels normaux sont simplement envoyés vers l’imprimante, séparés par des sauts de ligne.

L’élément [**Windows PointOfService API**](https://msdn.microsoft.com/library/windows/apps/Dn298071) vous offre une large part de cette fonctionnalité via les méthodes **Print()** ou **PrintLine()**. Toutefois, pour obtenir une mise en forme spécifique ou pour transmettre des commandes spécifiques, vous devez utiliser les commandes ESC/POS, développées en tant que chaînes et transmises à l’imprimante.

## Exemple utilisant des caractères en gras et de taille double

L’exemple ci-dessous vous explique comment utiliser les commandes ESC/POS pour imprimer les caractères en gras et de taille double. Notez que chaque commande est générée en tant que chaîne, puis insérée dans les appels de tâche d’impression.

```csharp
// … prior plumbing code removed for brevity
// this code assumed you've already created a receipt print job (printJob)
// and also that you've already checked the PosPrinter Capabilities to 
// verify that the printer supports Bold and DoubleHighDoubleWide print modes

const string ESC = "\u001B";
const string GS = "\u001D";
const string InitializePrinter = ESC + "@";
const string BoldOn = ESC + "E" + "\u0001";
const string BoldOff = ESC + "E" + "\0";
const string DoubleOn = GS + "!" + "\u0011";  // 2x sized text (double-high + double-wide)
const string DoubleOff = GS + "!" + "\0";

printJob.Print(InitializePrinter);
printJob.PrintLine("Here is some normal text.");
printJob.PrintLine(BoldOn + "Here is some bold text." + BoldOff);
printJob.PrintLine(DoubleOn + "Here is some large text." + DoubleOff);

printJob.ExecuteAsync();
```

Pour plus d’informations sur ESC/POS, notamment sur les commandes disponibles, consultez le [FAQ sur le système ESC/POS d’Epson](http://content.epson.de/fileadmin/content/files/RSD/downloads/escpos.pdf). Pour plus d’informations sur [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) et l’ensemble des fonctionnalités disponibles, consultez la page [Imprimante PointofService](https://msdn.microsoft.com/library/windows/apps/Mt426652) sur MSDN.





<!--HONumber=Jun16_HO4-->


