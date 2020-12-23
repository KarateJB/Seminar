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
    return name;
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

## Definition

| Title | Definition | Description |
|:-----:|:----------:|:------------|
| SLA | Service Level Agreement | The agreement you make with ur clients or users. |
| SLOs | Service Level Objectives | The agreement ur team must hit ti meet that agreement. |
| SLIs | Service Level Indicators | The real numbers on ur performance. |


## Preventive Maintenance Management

1. Decide Service goals
   - 99% Average Response time < 300ms
2. Estimate current state of our system
   - 99% Average Response time < 75ms
3. Decide Service levels and indicators
   - Green light: 99% Average Response time < 150ms
   - Yellow light: 99% Average Response time between 150ms ~ 300ms
   - Red light: 99% Average Response time > 200ms
4. Review the levels and indicators monthly/quarterly
   - List action items on Yellow/Red light events.


## A queue based SMS system example (for user registration and campain activities)

A: Frontend/APP (Sending the request)
B: Queue (Producing the SMS content)
C1: Backend (Consume the SMS content and Send the request to 3rd SMS service)
C2: 3rd SMS service

1. Monitor what happening in the system, except the relied 3rd services. (Thaz A, B, C)
2. Know the limit and bottlenecks from the indicators.
   - SLO: A + B + C <= 5 sec.
   - If A high = Sending the request too slow.
   - If B high = The messages are too many in Queue or Consuming too slow.
   - If C1 high = Handling the messages too slow.
   - If C2 high = The 3rd SMS service low performance for sending SMS.

### Solution 1: Message Worker Scaleout

Since C1 is high, the first solution is increasing the consumers.
However, this solution doubles the cost and the money was not spent on the cutting edge.


### Solution 2: Decrease Queue length

Descrese Queue legth by setting multiple Queues on different purposes. Such as Queue-1 is for user registration and Queue-2 is for Campaign activities.

Furthermore, set **Rate limit** while sending the requests to 3rd SMS service by different Queues.


### TOC(Thory Of Constraints)

Reference: [『高效率團隊』如何運用限制理論 (Theory of Constraints) 於軟體開發](https://medium.com/%E7%A7%91%E6%8A%80%E6%96%B0%E6%83%B3/%E9%AB%98%E6%95%88%E7%8E%87%E5%9C%98%E9%9A%8A-%E5%A6%82%E4%BD%95%E9%81%8B%E7%94%A8%E9%99%90%E5%88%B6%E7%90%86%E8%AB%96-theory-of-constraints-%E6%96%BC%E8%BB%9F%E9%AB%94%E9%96%8B%E7%99%BC-4617e36ba79a)

1. Find the bottleneck

   The previous stage usaully is being overflowed. 

2. Use the bottleneck wisely

   Dont stop the bottleneck stage, keep it on 100% usage.

3. Turn the resource from non-bottleneck stage to bottleneck stage

4. Improving to break the bottleneck

5. Back to step 1.


### Other solutions:

1. Control the data source. E.q. Help Desk uses the phone number to validate the user by manully calling him/her.

2. Temperatorly stop putting none-critical task into Queue. For example, if there are too many users register on the same time, we can stop sending Campaign SMS for a time.

3. Change the SLO. E.q. The Campaign SMS could be sent more than 5 sec.
   - User registration SMS <= 5 sec 
   - Campaign SMS <= 10 min

4. Enhance the performance of Workers.

5. Since a user has no patience on waiting more than 30 sec for the registration SMS. So we should not send the SMS that exceed 30 sec. 

### SLO vs COST

Always thinking about what cost will you take to meet the SLO.




# SQL Server 效能和你想的不一樣（許致學）

Some key settings:

- Cost threshold for parallelism = 25 ~ 50.
- max worker thread = 512 + (Core - 4) * 16.
- Disk formatted to 64K.
- Separate DB files to different files for disk IO improvement.


# 使用 .NET 5 實現美指期貨的量化交易策略（Will 保哥）

## Why

- Humanity weeakness: Greed and fear make investors do foolish thing
- Monitor the latest changes of market.

## How

- Indicators: the information of price changing in market.
- Signals: use indicators and codes to ouput if you should buy or sell. 
- Strategy: is the result of combining Indicators and Signals.

## Implementation with .NET 5

- Source Generators
- Record Types
- IAsyncEnumerable<T>
- System.Threading.Channels



# 蛻變 - Entity Framework Core 5.0（黃忠成）

## ORM vs Micro ORM

- ORM: Mapping, Translate Query(remoing the concept of SQL), Tracking, Relations
- Micro-ORM: Mapping, Simple CRUD plugin


## New features

- Mapping query result to object  (From Dapper)
- Install EF Core Power tools
- Simple logging
- dbContext.ChangeTracker.Clear() => Clean the copy of the DbContext.
- (From EF Core 3) Change proxies: the new tracking mode. useChangeTrackingProxies();
- SavePoint. Can rollback  to the previous SaveChanges (Only avaible to SQL Server now)
- DbContextFactory for DI
- Query Type, ToSqlQuery
- Compute columns (based on database)
- Global filter (suitable for soft-deletion or data permissions)
- Property Bag (for dynamic conditions)
- Split Query: Improve the performance of complext SQL in single query. Like a left join query, it will query the master table and detail table one by one and join them automatically.
- Inherit: TPH, TPT
- User Function Mapping
- Table Value Function (For SQL Server)
- SaveChange intercepter: SaveChange

## DBContext:
- Suggest to be short life.
- Need to be disposed for cleaning the entities in memory.
- Object Tracking: Context-Aware(Full-scan), if you dont need to modify, set "AsNoTracking"

## How to improve Performance

- Use Find instead of where
- Dont query twice
- Separate the query and update DbContext for NoTracking




# The Journey of C# Source Generator（Roberson Liou）

## What is Source Generator

- Only can generate C# code
- Lifecycle: C# -> Compile -> (Source generator: complication -> analyze code -> generate code -> Add generated code to complication) -> IL Code
- Could make CPU high

## Benefit:

- Turn runtime-reflection to compile-time
- Handle serializing file, such as .csv, .xml
- For partial class, partial method

## How to use:

- VS2019, .NET 5
- Debug lauch to debug , Syste,.Diagnostics.Debugger.Launch();
- Can be taged as auto-generated
- Have to restart VS to clean the cache
- Create .NET Standard 2.0 class library



# Reference

- [HACKMD](https://hackmd.io/@Study4/dotnetconf-2020/https%3A%2F%2Fdotnetconf2020.study4.tw%2F)
- [Slides](https://github.com/Study4/DotNetConfTaipei2020/tree/master/slides)