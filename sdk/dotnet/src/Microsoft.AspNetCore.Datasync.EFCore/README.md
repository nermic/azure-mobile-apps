# Microsoft Datasync Framework: Entity Framework Core Repository

The Microsoft Datasync Framework (previously Azure Mobile Apps) is a client-server protocol that allows a client application to synchronize table data from a service-based database. From a purely technical perspective, the protocol is based on [OData](https://www.odata.org) to allow searching, sorting, and paging of results, plus a version-aware CRUD (Create-Read-Update-Delete) web API.

This library is the Entity Framework Core support. It is used alongside the [Microsoft.AspNetCore.Datasync](https://www.nuget.org/packages?q=Microsoft.AspNetCore.Datasync) library.

## Preview Library

This library is provided in a "preview" state.  We do not feel it is ready for production use yet.

## Supported environments

This library support ASP.NET Core for .NET 5 and .NET 6.

## How to use

When creating a datasync controller, first, create a Data Transfer Object (DTO) that inherits from `EntityTableData`:

``` csharp
using Microsoft.AspNetCore.Datasync.EFCore;
using System.ComponentModel.DataAnnotations;

namespace TodoAppService.NET6.Db
{
    public class TodoItem : EntityTableData
    {
        [Required, MinLength(1)]
        public string Title { get; set; } = "";

        public bool IsComplete { get; set; }
    }
}
```

Add this model as a `DbSet<T>` to the `DbContext`:

``` csharp
using Microsoft.EntityFrameworkCore;

namespace TodoAppService.NET6.Db
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
        {
        }

        public DbSet<TodoItem> TodoItems => Set<TodoItem>();
    }
}
```

Then create a `TableController<T>` to handle the service requests. Set the repository to an instance of the  `EntityTableRepository<T>`:

``` csharp
using Microsoft.AspNetCore.Datasync;
using Microsoft.AspNetCore.Datasync.EFCore;
using Microsoft.AspNetCore.Mvc;
using TodoAppService.NET6.Db;

namespace TodoAppService.NET6.Controllers
{
    [Route("tables/todoitem")]
    public class TodoItemController : TableController<TodoItem>
    {
        public TodoItemController(AppDbContext context)
            : base(new EntityTableRepository<TodoItem>(context))
        {
        }
    }
}
```

For more information, please review [our documentation](https://azure.github.io/azure-mobile-apps/howto/server/aspnetcore/).

## Asking questions

Please ask questions either on [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-mobile-services) or [GitHub Discussions](https://github.com/Azure/azure-mobile-apps/discussions).

## Reporting bugs or requesting features

Please report bugs or request features through [GitHub Issues](https://github.com/Azure/azure-mobile-apps/issues).

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit [https://cla.opensource.microsoft.com](https://cla.opensource.microsoft.com).

When you submit a pull request, a CLA bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.