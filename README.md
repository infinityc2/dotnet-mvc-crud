# .Net MVC 

- ASP.NET Core Web Application
- Web Application (Model-View-Controller)


## Create MVC
1. create model
```C#
using System;
using System.ComponentModel.DataAnnotations;

namespace WebApplication.Models
{
    public class Movie
    {
        public int Id { get; set; }
        [StringLength(60, MinimumLength = 3)]
        [Required]
        public string Title { get; set; }

        [Display(Name = "Release Date")]
        [DataType(DataType.Date)]
        public DateTime ReleaseDate { get; set; }

        [Range(1, 100)]
        [DataType(DataType.Currency)]
        [Column(TypeName = "decimal(18, 2)")]
        public decimal Price { get; set; }

        [RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
        [Required]
        [StringLength(30)]
        public string Genre { get; set; }

        [RegularExpression(@"^[A-Z]+[a-zA-Z0-9""'\s-]*$")]
        [StringLength(5)]
        [Required]
        public string Rating { get; set; }
    }
}
```
2. Add NuGet Packages
```sh
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
3. Create a database context class
```C#
using Microsoft.EntityFrameworkCore;
using WebApplication.Models;

namespace WebApplication.Data
{
    public class MvcContext : DbContext
    {
        public MvcContext (DbContextOptions<MvcContext> options)
            : base(options)
        {
        }

        public DbSet<Movie> Movie { get; set; }
    }
}
```
4. Register the database context at <code>Startup.cs</code>
```C#
using WebApplication.Data;
using Microsoft.EntityFrameworkCore;
```
Startup.ConfigureServices
```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();

    services.AddDbContext<MvcContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MvcContext")));
} 
```
5. Add a database connection string
```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "MvcContext": "Server=(localdb)\\mssqllocaldb;Database=MvcContext-1;Trusted_Connection=True;MultipleActiveResultSets=true"
  } 
}
```
6. Scaffold
- Right Click on Controller Directory > Add > New Scaffold Item
- Select MVC Controller with views, using Entity Framework
7. Migration
```sh
Add-Migration InitialCreate
Update-Database
```