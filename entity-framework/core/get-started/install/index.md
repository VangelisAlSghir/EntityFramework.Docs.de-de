---
title: Installieren von Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 5ebc4edba07063ad5e77154adcde5f2664c0d748
ms.sourcegitcommit: 85d17524d8e022f933cde7fc848313f57dfd3eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55760521"
---
# <a name="installing-entity-framework-core"></a>Installieren von Entity Framework Core

## <a name="prerequisites"></a>Erforderliche Komponenten

* EF Core ist eine [.NET Standard 2.0](/dotnet/standard/net-standard)-Bibliothek. D.h., dass EF Core eine .NET-Implementierung erfordert, die .NET Standard 2.0 unterstützt. Auf EF Core kann auch von anderen .NET Standard 2.0-Bibliotheken verwiesen werden. 

* Sie können EF Core z.B. verwenden, um Apps für .NET Core zu entwickeln. Zum Erstellen von .NET Core-Apps ist das [.NET Core SDK](https://dotnet.microsoft.com/download) erforderlich. Optional können Sie auch eine Entwicklungsumgebung wie Visual Studio, Visual Studio für Mac oder Visual Studio Code verwenden. Weitere Informationen finden Sie unter [Erste Schritte mit .NET Core](/dotnet/core/get-started).

* Mit EF Core können Sie in Visual Studio unter Windows Anwendungen für .NET Framework 4.6.1 oder höher entwickeln. Die aktuelle Version von Visual Studio wird empfohlen. Wenn Sie eine ältere Version verwenden möchten, z.B. Visual Studio 2015, stellen Sie sicher, dass Sie den [NuGet-Client auf Version 3.6.0 upgraden](https://www.nuget.org/downloads), damit dieser mit .NET Standard 2.0-Bibliotheken funktioniert.

* EF Core kann auch in anderen .NET-Implementierungen, z.B. Xamarin oder .NET Native, ausgeführt werden. Diese Implementierungen haben jedoch Laufzeiteinschränkungen, die sich auf die Funktionsweise von EF Core in Ihrer App auswirken können. Weitere Informationen finden Sie unter [.NET implementations supported by EF Core (.NET-Implementierungen, die von EF Core unterstützt werden)](xref:core/platforms/index).

* Es kann zudem sein, dass unterschiedliche Datenbankanbieter bestimmte Datenbank-Engineversionen, .NET-Implementierungen oder Betriebssysteme erfordern. Achten Sie darauf, dass ein [EF Core-Datenbankanbieter](xref:core/providers/index) zur Verfügung steht, der eine für Ihre Anwendung geeignete Umgebung unterstützt.

## <a name="get-the-entity-framework-core-runtime"></a>Abrufen der Entity Framework Core-Runtime

Installieren Sie das NuGet-Paket für den Datenbankanbieter, den Sie verwenden möchten, um einer Anwendung EF Core hinzuzufügen.

Wenn Sie eine ASP.NET Core-Anwendung erstellen, müssen Sie keine In-Memory- und SQL Server-Anbieter installieren. Diese Anbieter sind neben der EF Core-Runtime in der aktuellen Versionen von ASP.NET Core enthalten.  

Verwenden Sie die .NET Core-CLI (Command Line Interface, Befehlszeilenschnittstelle), das Dialogfeld für den Visual Studio-Paket-Manager oder die Visual Studio-Paket-Manager-Konsole, um NuGet-Pakete zu installieren oder zu aktualisieren.

### <a name="net-core-cli"></a>.NET Core-CLI

* Verwenden Sie den folgenden .NET Core-CLI-Befehl über die Befehlszeile des Betriebssystems, um den EF Core-SQL Server-Anbieter zu installieren oder zu aktualisieren:

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Mit dem Modifizierer `-v` können Sie eine bestimmte Version im Befehl `dotnet add package` angeben. Wenn Sie beispielsweise EF Core 2.2.0-Pakete installieren möchten, fügen Sie `-v 2.2.0` an den Befehl an.

Weitere Informationen finden Sie unter [Tools für die .NET Core-Befehlszeilenschnittstelle](/dotnet/core/tools/).

### <a name="visual-studio-nuget-package-manager-dialog"></a>Dialogfeld für den NuGet-Paket-Manager in Visual Studio

* Klicken Sie im Visual Studio-Menü auf **Projekt > NuGet-Pakete verwalten**.

* Klicken Sie auf **Durchsuchen** oder die Registerkarte **Updates**.

* Wählen Sie das `Microsoft.EntityFrameworkCore.SqlServer`-Paket aus, und bestätigen Sie, um den SQL Server-Anbieter zu installieren oder zu aktualisieren.

Weitere Informationen finden Sie im [Dialogfeld des NuGet-Paket-Managers](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>NuGet-Paket-Manager-Konsole (Visual Studio)

* Wählen Sie im Visual Studio-Menü **Extras > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.

* Führen Sie den folgenden Befehl in der Paket-Manager-Konsole aus, um den SQL Server-Anbieter zu installieren:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Verwenden Sie den `Update-Package`-Befehl, um den Anbieter zu aktualisieren.

* Um eine bestimmte Version anzugeben, können Sie den Modifizierer `-Version` verwenden. Wenn Sie beispielsweise EF Core 2.2.0-Pakete installieren möchten, fügen Sie `-Version 2.2.0` an den Befehl an.

Weitere Informationen finden Sie im Artikel zur [Paket-Manager-Konsole](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Installieren der Entity Framework Core-Tools

Sie können Tools installieren, die Tasks im Zusammenhang mit EF Core in Ihrem Projekt ausführen, z.B. das Erstellen und Durchführen von Datenbankmigrationen oder das Erstellen eines EF Core-Modells basierend auf einer vorhandenen Datenbank.

Es sind zwei Sätze von Tools verfügbar:

* Die [.NET Core-CLI-Tools](xref:core/miscellaneous/cli/dotnet) können unter Windows, Linux und macOS verwendet werden. Diese Befehle beginnen mit `dotnet ef`. 

* Die [Tools der Paket-Manager-Konsole (PMC-Tools)](xref:core/miscellaneous/cli/powershell) werden in Visual Studio unter Windows ausgeführt. Diese Befehlen beginnen mit einem Verb, z.B. `Add-Migration`, `Update-Database`.

Obwohl Sie die `dotnet ef`-Befehle auch über die Paket-Manager-Konsole verwenden können, wird empfohlen, die Tools der Paket-Manager-Konsole zu verwenden, wenn Sie Visual Studio nutzen:

* Sie werden automatisch mit dem aktuellen in der PMC in Visual Studio ausgewählten Projekt ausgeführt, ohne dass manuell zwischen den Verzeichnissen gewechselt werden muss.  

* Nach Abschluss des Befehls öffnen sie automatisch Dateien, die von den Befehlen in Visual Studio generiert wurden.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>Installieren von .NET Core-CLI-Tools

.NET Core-CLI-Tools erfordern das .NET Core SDK, das bereits unter [Erforderliche Komponenten](#prerequisites) erwähnt wurde.

Die `dotnet ef`-Befehle sind in den aktuellen Versionen des .NET Core SDK enthalten. Sie müssen jedoch das `Microsoft.EntityFrameworkCore.Design`-Paket installieren, um diese Befehle für ein bestimmtes Projekt nutzen zu können:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Für ASP.NET Core-Apps ist dieses Paket automatisch enthalten.

> [!IMPORTANT]      
> Verwenden Sie immer die Toolpaketversion, die der Hauptversion der Runtimepakete entspricht.

### <a name="get-the-package-manager-console-tools"></a>Abrufen der Tools für die Paket-Manager-Konsole

Installieren Sie das `Microsoft.EntityFrameworkCore.Tools`-Paket, um die PMC-Tools für EF Core zu installieren. In Visual Studio sieht dies z.B. folgendermaßen aus:

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

Für ASP.NET Core-Apps ist dieses Paket automatisch enthalten.

## <a name="upgrading-to-the-latest-ef-core"></a>Upgrade auf die aktuelle Version von EF Core

* Mit jedem Release von EF Core veröffentlichen wir auch eine neue Version der Anbieter, die Teil des EF Core-Projekts sind, u.a. Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite und Microsoft.EntityFrameworkCore.InMemory. Führen Sie ein Upgrade auf die neue Version eines Anbieters durch, um alle Verbesserungen nutzen zu können. 

* EF Core ist neben den SQL Server- und In-Memory-Anbietern in den aktuellen Versionen von ASP.NET Core enthalten. Führen Sie immer ein Upgrade von ASP.NET Core durch, um eine vorhandene ASP.NET Core-Anwendung auf eine neuere Version von EF Core zu upgraden.

* Wenn Sie eine Anwendung aktualisieren müssen, die einen Datenbankanbieter eines Drittanbieters verwendet, sollten Sie immer nach einem Update des Anbieters suchen, der mit der von Ihnen gewünschten Version von EF Core kompatibel ist. Datenbankanbieter für vorherige Versionen sind nicht mit Version 2.0 der EF Core-Runtime kompatibel.

* Drittanbieter für EF Core veröffentlichen normalerweise keine Patchversionen mit der EF Core-Runtime. Fügen Sie einen direkten Verweis auf einzelne Komponenten der EF Core-Runtime hinzu, um eine Anwendung upzugraden, die einen Drittanbieter verwendet, um eine Version von EF Core zu patchen, z.B. Microsoft.EntityFrameworkCore und Microsoft.EntityFrameworkCore.Relational.

* Wenn Sie ein Upgrade einer vorhandenen Anwendung auf die aktuelle Version von EF Core durchführen, müssen einige Verweise auf ältere EF Core-Pakete eventuell manuell entfernt werden:

  * Entwurfszeitpakete von Datenbankanbietern wie `Microsoft.EntityFrameworkCore.SqlServer.Design` sind ab EF Core 2.0 nicht mehr erforderlich und werden nicht unterstützt. Sie werden bei der Durchführung eines Upgrades anderer Pakete jedoch nicht automatisch entfernt.

  * Die .NET-CLI-Tools sind im .NET SDK ab Version 2.1 enthalten. Der Verweis auf das Paket kann also aus der Projektdatei entfernt werden:

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* .NET Framework-Anwendungen müssen eventuell angepasst werden, um mit der .NET Standard 2.0-Bibliothek zu funktionieren:

  * Bearbeiten Sie die Projektdatei, und stellen Sie sicher, dass der folgende Eintrag in der initialen Eigenschaftengruppe angezeigt wird:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Achten Sie bei Testprojekten auch darauf, dass der folgende Eintrag vorhanden ist:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
