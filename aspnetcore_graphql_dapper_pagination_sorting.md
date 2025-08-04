# ASP.NET Core + GraphQL + Dapper 範例：分頁與欄位排序

這份文件說明如何在 ASP.NET Core 專案中，整合 GraphQL 與 Dapper，並實作分頁與欄位可選的排序功能。資料來源為資料庫，查詢透過 Dapper 執行，商業邏輯獨立封裝於 Service 層，GraphQL Resolver 僅負責轉發查詢請求。

---

## ✅ 功能目標

- 支援分頁：`PageIndex`、`PageSize`
- 支援動態排序：`SortBy`、`IsDescending`
- 商業邏輯集中於 Service 層
- 使用 Dapper 存取資料庫

---

## 📦 需要的 NuGet 套件

```bash
Install-Package HotChocolate.AspNetCore
Install-Package HotChocolate.Data
Install-Package Dapper
Install-Package Microsoft.Data.SqlClient
```

---

## 📁 專案結構建議

```
/GraphQL
  └── Query.cs                ← GraphQL Resolver
/Services
  └── IProductService.cs
  └── ProductService.cs      ← 商業邏輯
/Repositories
  └── IProductRepository.cs
  └── ProductRepository.cs   ← Dapper 查詢
/Models
  └── Product.cs
  └── ProductQueryInput.cs   ← 查詢條件
```

---

## 🧾 查詢輸入類別

```csharp
public class ProductQueryInput
{
    public int PageIndex { get; set; } = 1;
    public int PageSize { get; set; } = 10;
    public string? SortBy { get; set; }
    public bool IsDescending { get; set; } = false;
}
```

---

## 📦 Model 類別

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = default!;
    public decimal Price { get; set; }
    public int Stock { get; set; }
}
```

---

## 🧠 Service 層（含商業邏輯）

### Interface
```csharp
public interface IProductService
{
    Task<IEnumerable<Product>> GetPagedProductsAsync(ProductQueryInput input);
}
```

### 實作
```csharp
public class ProductService : IProductService
{
    private readonly IProductRepository _repo;

    public ProductService(IProductRepository repo)
    {
        _repo = repo;
    }

    public async Task<IEnumerable<Product>> GetPagedProductsAsync(ProductQueryInput input)
    {
        if (input.PageSize > 100)
            input.PageSize = 100;

        return await _repo.GetProductsPagedAsync(input);
    }
}
```

---

## 🗃️ Repository 層（使用 Dapper）

### Interface
```csharp
public interface IProductRepository
{
    Task<IEnumerable<Product>> GetProductsPagedAsync(ProductQueryInput input);
}
```

### 實作
```csharp
public class ProductRepository : IProductRepository
{
    private readonly IDbConnection _db;

    public ProductRepository(IConfiguration config)
    {
        _db = new SqlConnection(config.GetConnectionString("DefaultConnection"));
    }

    public async Task<IEnumerable<Product>> GetProductsPagedAsync(ProductQueryInput input)
    {
        var validSortFields = new[] { "Id", "Name", "Price", "Stock" };
        var sortBy = validSortFields.Contains(input.SortBy) ? input.SortBy : "Id";
        var order = input.IsDescending ? "DESC" : "ASC";

        var offset = (input.PageIndex - 1) * input.PageSize;

        var sql = $@"
            SELECT Id, Name, Price, Stock
            FROM Products
            ORDER BY {sortBy} {order}
            OFFSET @Offset ROWS FETCH NEXT @PageSize ROWS ONLY";

        return await _db.QueryAsync<Product>(sql, new { Offset = offset, PageSize = input.PageSize });
    }
}
```

---

## 🔍 GraphQL Resolver

```csharp
public class Query
{
    public async Task<IEnumerable<Product>> GetProductsAsync(
        ProductQueryInput input,
        [Service] IProductService service)
    {
        return await service.GetPagedProductsAsync(input);
    }
}
```

---

## 🔧 Startup 設定

```csharp
builder.Services
    .AddScoped<IProductService, ProductService>()
    .AddScoped<IProductRepository, ProductRepository>();

builder.Services
    .AddGraphQLServer()
    .AddQueryType<Query>()
    .AddType<ProductQueryInput>();
```

---

## 🧪 測試用 GraphQL Query 範例

```graphql
query {
  products(input: {
    pageIndex: 1,
    pageSize: 5,
    sortBy: "Price",
    isDescending: true
  }) {
    id
    name
    price
    stock
  }
}
```

---

## ✅ 小結

| 層級       | 責任                 |
|------------|----------------------|
| GraphQL    | 接收輸入、轉交 Service |
| Service    | 處理商業邏輯           |
| Repository | 用 Dapper 查資料       |

---

如需進一步支援：
- 加入總筆數與分頁回傳物件（如 `PagedResult<T>`）
- 支援條件篩選（如名稱模糊查詢）

請另提出需求，我可以擴充此範例。

