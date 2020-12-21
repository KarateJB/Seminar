## ASP.NET Core 5.0 新功能演繹 （奚江華）

### New features

- Model binding on `record` type.
- Model binfing on `Datetime` type with UTC support.
- WebApi template supports Swagger in default.
  Disable it like this, 
  ```s
  $ dotnet new webapi --no-openapi true 
  ```

### init setter

The property will be readonly after initialization.

```csharp
prop {get; set;}
```

### record type

- Immutable reference type.
- Canbe used on Data|API|View Model. But we cannot transform between `record` and `class`. Use something like [AutoMapper](https://automapper.org/) in this case.
- Can simplify DB Seed data codes.
- Can define Data annotation on it. Such as `record Person = new ([Required][MaxLegth(10)] string Name);`.
- Create a `record` type in this easy way:
  ```csharp
  public record Product(int Id, string Name);
  Product p1 = new (1, "PC"); // Target-typed new expressions
  ```
- Deep-clone the `record` type object
  ```csharp
  Product p2 = p1 with { name="Super PC"}; 
  ```

---

## C# Pattern Matching 演進史（Bill Chung）


## 非同步系統的服務水準保證；淺談非同步系統的 SLO 設計（Andrew Wu）


## 微型任務編排器 - 以 Process Pool 為例（Steven Tsai）


## SQL Server 效能和你想的不一樣（許致學）


## 使用 .NET 5 實現美指期貨的量化交易策略（Will 保哥）


## AKS 好朋友（Inca）


## 蛻變 - Entity Framework Core 5.0（黃忠成）


## 不會 Javascript 沒關係，用 Blazor 來解決前端需求 - 成為 Full Stack .NET 開發者吧（Alan Tsai）


## The Journey of C# Source Generator（Roberson Liou）

