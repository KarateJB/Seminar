# ASP.NET Core 5.0 新功能演繹 （奚江華）

## New features

- Model binding on `record` type.
- Model binfing on `Datetime` type with UTC support.
- WebApi template supports Swagger in default.
  Disable it like this, 
  ```s
  $ dotnet new webapi --no-openapi true 
  ```

## init setter

The property will be readonly after initialization.

```csharp
prop {get; set;}
```

## record type

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

# C# Pattern Matching 演進史（Bill Chung）


## Tuple pattern

For example, define a new tuple containing `state`, `operation`, `is valid key` and then check them inside `switch` as following,

```csharp
var newState = (state, operation, key.IsValid) switch
{
  (State.Opened, Operation.Close, _)      => State.Closed,
  (State.Opened, Operation.Open, _)       => throw new Exception(
    "Can't open an opened door"),
  (State.Opened, Operation.Lock, true)    => State.Locked,
  (State.Locked, Operation.Open, true)    => State.Opened,
  (State.Closed, Operation.Open, false)   => State.Locked,
  (State.Closed, Operation.Lock, true)    => State.Locked,
  (State.Closed, Operation.Close, _)      => throw new Exception(
    "Can't close a closed door"),
  _ => state
};
```

## Position pattern

```csharp
Shape shape = new Rectangle
{
  Width = 100,
  Height = 100,
  Point = new Point { X = 0, Y = 100 }
};

var result = shape switch
{
  Rectangle (100, 100, null) => "Found 100x100 rectangle without a point",
  Rectangle (100, 100, _) => "Found 100x100 rectangle",
  _ => "Different, or null shape"
};
```


## Relational pattern

### Sample 1.

- Before

```csharp
class Rectangle 
 {
     public double Width { get; set; }
     public double Height { get; set; }
     public double GetArea()
     {
         return Width * Height;
     }
 }


static string GetSize(Rectangle  rect) => rect.GetArea() switch
 {       
     double area when area < 10 => "Small",
     double area when area >= 100 => "Big",            
     _ => "Middle rectangle"
 };
```

- C# 9

```csharp
static string GetSize(Rectangle rect) => rect.GetArea() switch
{
    < 10 => "Small",
    >= 100 => "Large",
    _ => "Middle"
};
```

### Sample 2.

- Before

```csharp
if (p is Student { Gender: Gender.Male, Name: string name, Score: int score } && score > 70)
{
    return name;
}
```

- C# 9

```csharp
if (p is Student { Gender: Gender.Male, Name: string name, Score: > 70 })
{
    yield return name;
}
```

## Logical pattern

```csharp
static string GetSize(Rectangle rect) => rect.GetArea() switch
{
    >= 5 and <= 10 => "Small",
    >= 100 and <= 1000 => "Large",
    > 1000 or < 5 => "Extreme small or large"
    _ => "Middle"
};
```

## No pattern

Filter the instances that are not `Student` type,

```csharp
var teachers = people.Where(x => x is not Student);
```

Check if the instance is not null,

```csharp
var names = people.Where(x => x.Name is not null).Select(x => x.Name);
```


## Simple type pattern

-- Before

We must use [Discards](https://docs.microsoft.com/en-us/dotnet/csharp/discards) to skip the useless variables as following,

```csharp
static string GetShape(IShape shape) => shape switch
{
    null => "null",
    Rectangle _ => "Rectangle",
    Circle _=> "Circle",
    _ => "??"
};
```

Now we can skip the [Discards](https://docs.microsoft.com/en-us/dotnet/csharp/discards).

```csharp
static string GetShape(IShape shape) => shape switch
{
    null => "null",
    Rectangle => "Rectangle",
    Circle => "Circle",
    _ => "??"
};
```



# 非同步系統的服務水準保證；淺談非同步系統的 SLO 設計（Andrew Wu）


# 微型任務編排器 - 以 Process Pool 為例（Steven Tsai）


# SQL Server 效能和你想的不一樣（許致學）


# 使用 .NET 5 實現美指期貨的量化交易策略（Will 保哥）


# AKS 好朋友（Inca）


# 蛻變 - Entity Framework Core 5.0（黃忠成）


# 不會 Javascript 沒關係，用 Blazor 來解決前端需求 - 成為 Full Stack .NET 開發者吧（Alan Tsai）


# The Journey of C# Source Generator（Roberson Liou）

