---
title: Entity Framework Core Tools-Referenz – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 9fcb452c2798a3d07e39cbcc3c34629dca4394ff
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447117"
---
# <a name="entity-framework-core-tools-reference"></a>Entity Framework Core Tools-Referenz

Die Entity Framework Core Tools helfen zur Entwurfszeit bei Entwicklungsaufgaben. Diese werden hauptsächlich verwendet, um Migrationen zu verwalten und ein Gerüst für `DbContext` und Entitätstypen durch Reverse Engineering eines Datenbankschemas zu erstellen.

* Die [EF Core Paket-Manager-Konsolentools](powershell.md)) werden in der [Paket-Manager-Konsole](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio ausgeführt. Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.

* Die [EF Core .NET-Befehlszeilentools (CLI-Tools)](dotnet.md) stellen eine Erweiterung der plattformübergreifenden [.NET Core CLI-Tools](https://docs.microsoft.com/dotnet/core/tools/) dar. Für diese Tools ist ein .NET Core-SDK-Projekt (mit `Sdk="Microsoft.NET.Sdk"` oder einem ähnlichen SDK in der Projektdatei) erforderlich.

Beide Tools bieten dieselbe Funktionalität. Für die Entwicklung in Visual Studio empfehlen wir die Verwendung von **Paket-Manager-Konsolentools**, da diese ein besseres ganzheitliches Erlebnis bieten.

## <a name="next-steps"></a>Nächste Schritte

* [EF Core-Paket-Manager-Konsolentools – Referenz](powershell.md)
* [EF Core .NET CLI-Tools – Referenz](dotnet.md)