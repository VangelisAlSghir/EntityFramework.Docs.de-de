---
title: Laden zugehöriger Daten – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: e98e2e601203db7ea3d3344ddc7b7e0aff7f2143
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562519"
---
# <a name="loading-related-data"></a>Laden zugehöriger Daten

Mit Entity Framework Core können Sie die Navigationseigenschaften in Ihrem Modell zum Laden zugehöriger Entitäten verwenden. Es gibt drei allgemeine O/RM-Muster für das Laden zugehöriger Daten.
* **Eager Loading** bedeutet, dass die zugehörigen Daten als Teil der ersten Abfrage aus der Datenbank geladen werden.
* **Explizites Laden** bedeutet, dass die zugehörigen Daten zu einem späteren Zeitpunkt explizit aus der Datenbank geladen werden.
* **Lazy Loading** bedeutet, dass die zugehörigen Daten bei Zugriff auf die Navigationseigenschaft transparent aus der Datenbank geladen werden.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.

## <a name="eager-loading"></a>Eager Loading

Sie können mit der `Include`-Methode zugehörige Daten angeben, die in den Abfrageergebnissen enthalten sein sollen. Im folgenden Beispiel weisen die in den Ergebnissen zurückgegebenen Blogs ihre `Posts`-Eigenschaft auf, die mit den zugehörigen Beiträgen aufgefüllt wurden.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core korrigiert automatisch Navigationseigenschaften zur Korrektur für alle anderen Entitäten, die zuvor in die Kontextinstanz geladen wurden. Auch wenn Sie die Daten für eine Navigationseigenschaft nicht explizit eingeschlossen haben, kann die Eigenschaft folglich immer noch aufgefüllt werden, wenn einige oder alle zugehörigen Entitäten zuvor geladen wurden.


Sie können zugehörigen Daten aus mehreren Beziehungen in einer einzelnen Abfrage einschließen.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Einschließen mehrerer Ebenen

Sie können mit der `ThenInclude`-Methode einen Drilldown für Beziehungen ausführen, um mehrere Ebenen zugehöriger Daten einzuschließen. Im folgenden Beispiel werden sämtliche Blogs, die zugehörigen Beiträge und die Autoren der einzelnen Beiträge geladen.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Aktuelle Versionen von Visual Studio bieten falsche Codevervollständigungsoptionen an. Diese können verursachen, dass bei Verwendung der `ThenInclude`-Methode gemäß der Navigationseigenschaft einer Sammlung korrekte Ausdrücke mit Syntaxfehler gekennzeichnet werden. Dies ist ein Symptom für einen unter https://github.com/dotnet/roslyn/issues/8237 nachverfolgten IntelliSense-Fehler. Diese unbegründeten Syntaxfehler können ohne Bedenken ignoriert werden, solange der Code korrekt ist und erfolgreich kompiliert werden kann. 

Sie können mehrere Aufrufe mit `ThenInclude` verbinden, um mit dem Einschließen weiterer Ebenen zugehöriger Daten fortzufahren.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Sie können all dies kombinieren, um zugehörige Daten aus mehreren Ebenen und mehreren Stämmen in derselben Abfrage einzuschließen.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Für eine der Entitäten, die eingeschlossen wird, sollten Sie mehrere zugehörige Entitäten einschließen. Beispiel: Beim Abfragen von `Blogs` schließen Sie `Posts` ein und möchten anschließend `Author` und `Tags` von `Posts` einschließen. Hierzu müssen Sie die einzelnen Includepfade beginnend beim Stamm angeben. Beispiel: `Blog -> Posts -> Author` und `Blog -> Posts -> Tags`. Dies bedeutet nicht, dass Sie redundante Verknüpfungen erhalten. In den meisten Fällen konsolidiert EF die Verknüpfungen beim Generieren von SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Einschließen in abgeleiteten Typen

Sie können zugehörige Daten aus Navigationen einschließen, die nur in einem abgeleiteten Typ mit `Include` und `ThenInclude` definiert werden. 

Betrachten Sie das folgende Modell:

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

Inhalte der `School`-Navigation für alle Personen, die Studenten sind, können mit einer Reihe von Mustern vorzeitig geladen werden:

- Mit cast
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- Mit dem Operator `as`
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- Mit einer Überladung von `Include`, die Parameter vom Typ `string` akzeptiert
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a>Ignorierte Include-Operatoren

Wenn Sie die Abfrage so ändern, dass keine Instanzen des Entitätstyps mehr zurückgegeben werden, mit dem die Abfrage gestartet wurde, werden die Include-Operatoren ignoriert.

Im folgenden Beispiel basieren die Include-Operatoren auf dem Operator `Blog`. Die Abfrage wird jedoch dann mit dem Operator `Select` geändert, damit ein anonymer Typ zurückgegeben wird. In diesem Fall haben die Include-Operatoren keine Auswirkungen.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

EF Core protokolliert standardmäßig eine Warnung, wenn Include-Operatoren ignoriert werden. Weitere Informationen zum Anzeigen von Protokollierungsausgaben finden Sie unter [Protokollierung](../miscellaneous/logging.md). Sie können das Verhalten, wenn ein Include-Operator ignoriert wird, auslösen oder nichts unternehmen. Dies geschieht, wenn die Optionen für Ihren Kontext – normalerweise in `DbContext.OnConfiguring` oder `Startup.cs` – bei Verwendung von ASP.NET Core eingerichtet werden.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Explizites Laden

> [!NOTE]  
> Dieses Feature wurde in EF Core 1.1 eingeführt.

Sie können eine Navigationseigenschaft über die API `DbContext.Entry(...)` explizit laden.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Sie können eine Navigationseigenschaft auch explizit laden, indem Sie eine separate Abfrage ausführen, die die zugehörigen Entitäten zurückgibt. Wenn die Änderungsnachverfolgung aktiviert ist, legt EF Core beim Laden einer Entität die Navigationseigenschaften der neu geladenen Entität automatisch so fest, dass diese auf bereits geladene Entitäten verweisen, und die Navigationseigenschaften bereits geladener Entitäten so, dass diese auf die neu geladene Entität verweisen.

### <a name="querying-related-entities"></a>Abfragen zugehöriger Entitäten

Sie können auch eine LINQ-Abfrage abrufen, welche die Inhalte einer Navigationseigenschaft darstellt.

Dadurch können Sie Aktionen, wie z.B. das Ausführen eines Aggregatoperators, über die zugehörigen Entitäten ausführen, ohne diese in den Speicher laden zu müssen.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Darüber hinaus können Sie filtern, welche zugehörigen Entitäten in den Speicher geladen werden.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Lazy Loading

> [!NOTE]  
> Dieses Feature wurde in EF Core 2.1 eingeführt.

Die einfachste Möglichkeit für die Verwendung von Lazy Loading besteht in der Installation des Pakets [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) und der Aktivierung dieses Pakets durch Aufrufen von `UseLazyLoadingProxies`. Beispiel:
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Oder bei Verwendung von AddDbContext:
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
EF Core aktiviert Lazy Loading anschließend für beliebige überschreibbare Navigationseigenschaften – diese müssen `virtual` und in einer Klasse enthalten sein, aus der sie geerbt werden können. In den folgenden Entitäten wird beispielsweise Lazy Loading für die Navigationseigenschaften `Post.Blog` und `Blog.Posts` durchgeführt.
```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a>Lazy Loading ohne Proxys

Lazy Loading-Proxys funktionieren, indem der Dienst `ILazyLoader` in eine Entität eingefügt wird, wie unter [Entitätstypenkonstruktoren](../modeling/constructors.md) beschrieben. Beispiel:
```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Es ist nicht erforderlich, dass aus Entitätstypen vererbt werden kann, oder dass Navigationseigenschaften „virtual“ sind. Mit `new` erstellte Entitätsinstanzen können Lazy Loading ausführen, sobald sie einem Kontext angefügt wurden. Erforderlich ist ein Verweis auf den `ILazyLoader`-Dienst, der im [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/)-Paket definiert ist. Dieses Paket enthält einen minimalen Satz von Typen, sodass es kaum Auswirkungen davon gibt. Um jedoch die Abhängigkeit von EF Core-Paketen in den Entitätstypen vollständig zu vermeiden, ist es möglich, die Methode `ILazyLoader.Load` als Delegat einzufügen. Beispiel:
```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Der oben stehende Code verwendet eine `Load`-Erweiterungsmethode, um die Verwendung des Delegaten sauberer zu gestalten:
```csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> Der Konstruktorparameter für den Lazy Loading-Delegaten muss als „lazyLoader“ bezeichnet werden. Eine Konfiguration für die Verwendung eines anderen Namens ist für ein zukünftiges Release geplant.

## <a name="related-data-and-serialization"></a>Zugehörige Daten und Serialisierung

Da EF Core Navigationseigenschaften automatisch korrigiert, können sich in Ihrem Objektgraph Zyklen ergeben. Das Laden eines Blogs und der zugehörigen Beiträge führt beispielsweise zu einem Blogobjekt, das auf eine Sammlung von Beiträgen verweist. Die einzelnen Beiträge verweisen wieder auf den Blog.

Einige Serialisierungsframeworks lassen solche Zyklen nicht zu. Json.NET löst beispielsweise folgende Ausnahme aus, wenn ein Zyklus erkannt wird.

> Newtonsoft.Json.JsonSerializationException: Für die Eigenschaft „Blog“ mit dem Typ „MyApplication.Models.Blog“ wurde eine auf sich selbst verweisende Schleife erkannt.

Wenn Sie ASP.NET Core verwenden, können Sie Json.NET so konfigurieren, dass im Objektgraph gefundene Zyklen ignoriert werden. Dies erfolgt in der Methode `ConfigureServices(...)` in `Startup.cs`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
