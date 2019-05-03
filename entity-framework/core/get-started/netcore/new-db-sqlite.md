---
title: Erste Schritte in .NET Core – Neue Datenbank – EF Core
author: rick-anderson
ms.author: riande
description: Erste Schritte mit .NET Core unter Verwendung von Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: a0df80a8fe96be4f8cc3177919e2b087e14cb49c
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325326"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Erste Schritte mit EF Core in einer .NET Core-Konsolen-App mit einer neuen Datenbank

In diesem Tutorial erstellen Sie eine .NET Core-Konsolen-App, die Datenzugriff auf eine SQLite-Datenbank mithilfe von Entity Framework Core ausführt. Sie verwenden Migrationen, um die Datenbank aus dem Modell zu erstellen. Siehe [ASP.NET Core – Neue Datenbank](xref:core/get-started/aspnetcore/new-db) für eine Visual Studio-Version mit Verwendung von ASP.NET Core MVC.

[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Erforderliche Komponenten

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Erstellt ein neues Projekt

* Erstellen Sie ein neues Konsolenprojekt:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a>Ändern des aktuellen Verzeichnisses

In den nachfolgenden Schritten werden `dotnet`-Befehle für die Anwendung ausgegeben.

* So ändern Sie das aktuelle Verzeichnis in das Verzeichnis der Anwendung

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a>Installieren von Entity Framework Core

Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter. In dieser exemplarischen Vorgehensweise wird SQLite verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* Installieren Sie „Microsoft.EntityFrameworkCore.Sqlite“ und „Microsoft.EntityFrameworkCore.Design“.

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* Führen Sie `dotnet restore` aus, um die neuen Pakete zu installieren.

## <a name="create-the-model"></a>Erstellen des Modells

Definieren Sie einen Kontext und Entitätsklassen für Ihr Modell.

* Erstellen Sie eine neue Datei *Model.cs* mit den folgenden Inhalten.

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Tipp: In einer echten Anwendung platzieren Sie jede Klasse in einer separaten Datei und fügen die Verbindungszeichenfolge in eine Konfigurationsdatei oder Umgebungsvariable ein. Der Einfachheit halber sind in diesem Tutorial alle Elemente in einer Datei enthalten.

## <a name="create-the-database"></a>Erstellen der Datenbank

Da Sie jetzt über ein Modell verfügen, können Sie mithilfe von [Migrationen](xref:core/managing-schemas/migrations/index) eine Datenbank erstellen.

* Führen Sie `dotnet ef migrations add InitialCreate` aus, um per Gerüstbau eine Migration einzurichten und den anfänglichen Tabellensatz für das Modell zu erstellen.
* Führen Sie `dotnet ef database update` aus, um die neue Migration auf die Datenbank anzuwenden. Mit diesem Befehl wird die Datenbank erstellt, bevor Migrationen angewendet werden.

Die SQLite-Datenbank *blogging.db* ist das Projektverzeichnis.

## <a name="use-the-model"></a>Verwenden des Modells

* Öffnen Sie die Datei *Program.cs*, und ersetzen Sie den Inhalt durch den folgenden Code:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testen Sie die App über die Konsole. Weitere Informationen zum Ausführen der App aus Visual Studio finden Sie im [Visual Studio-Hinweis](#vs).

  `dotnet run`

  Ein Blog wird in der Datenbank gespeichert, und die Details zu allen Blogs werden an die Konsole ausgegeben.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Ändern des Modells:

- Wenn Sie Änderungen am Modell vornehmen, können Sie mithilfe des `dotnet ef migrations add`-Befehls per Gerüstbau eine neue [Migration](xref:core/managing-schemas/migrations/index) erstellen. Nachdem Sie den Gerüstcode überprüft (und alle erforderlichen Änderungen vorgenommen) haben, können Sie mit dem Befehl `dotnet ef database update` Schemaänderungen auf die Datenbank anwenden.
- EF Core verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.
- Die SQLite-Datenbankengine unterstützt bestimmte Schemaänderungen nicht, die von den meisten anderen relationalen Datenbanken unterstützt werden. Beispielsweise wird der `DropColumn`-Vorgang nicht unterstützt. EF Core-Migrationen generieren Code für diese Vorgänge. Wenn Sie aber versuchen, diese auf eine Datenbank anzuwenden oder ein Skript zu generieren, löst EF Core Ausnahmen aus. Siehe [SQLite-Einschränkungen](../../providers/sqlite/limitations.md). Für eine Neuentwicklung können Sie erwägen, bei Modelländerungen die Datenbank zu verwerfen und eine neue Datenbank zu erstellen, anstatt Migrationen zu verwenden.

<a name="vs"></a>
### <a name="run-from-visual-studio"></a>Ausführen aus Visual Studio

Um dieses Beispiel von Visual Studio aus auszuführen, müssen Sie das Arbeitsverzeichnis manuell als Stammverzeichnis für das Projekt festlegen. Wenn Sie das Arbeitsverzeichnis nicht festlegen, wird der Ausnahmefehler `Microsoft.Data.Sqlite.SqliteException` ausgegeben: `SQLite Error 1: 'no such table: Blogs'`.

So legen Sie das Arbeitsverzeichnis fest

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Eigenschaften** aus.
* Wählen Sie im linken Bereich die Registerkarte **Debuggen** aus.
* Legen Sie **Arbeitsverzeichnis** auf das Projektverzeichnis fest.
* Speichern Sie die Änderungen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Tutorial: Erste Schritte mit EF Core in ASP.NET Core mit einer neuen Datenbank mithilfe von SQLite](xref:core/get-started/aspnetcore/new-db)
* [Tutorial: Erste Schritte mit Razor Pages in ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Tutorial: Razor Pages mit Entity Framework Core in ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
