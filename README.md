# Hania.Mapper

[![Build status](https://ci.appveyor.com/api/projects/status/q261l3sbokafmx1o/branch/master?svg=true)](https://www.nuget.org/packages/HaniaMapper/)
[![NuGet](http://img.shields.io/nuget/v/Hania.Mapper.svg)](https://www.nuget.org/packages/HaniaMapper/)
[![Author](https://img.shields.io/badge/Author-Akbar%20Ahmadi%20Saray-brightgreen.svg)](https://www.nuget.org/packages/HaniaMapper/)
[![Company](https://img.shields.io/badge/Company-Http%3A%2F%2FHaniaGroup.ir-orange.svg)](https://www.nuget.org/packages/HaniaMapper/)


### What is HaniaMapper?

HaniaMapper is a simple little library built to solve a deceptively complex problem - getting rid of code that mapped one object to another. This type of code is rather dreary and boring to write, so why not invent a tool to do it for us?


### Where can I get it?

First, [install NuGet](http://docs.nuget.org/docs/start-here/installing-nuget). Then, install [HaniaMapper](https://www.nuget.org/packages/HaniaMapper/) from the package manager console:

```
PM> Install-Package HaniaMapper 
```


### How do I get started?

#### First, Create Book Model Class 

```csharp
using System;

namespace HaniaMapperSample
{
  public  class Book
  {

    public int Id { get; set; }
    public string Title { get; set; }
    public string Discreption { get; set; }
    public DateTime RegisterDate { get; set; }

    public int AuthorId { get; set; }
    public Author Author { get; set; }
  }
}
```

And Author Model Class


```csharp
using System;

namespace HaniaMapperSample
{
  public class Author
  {
    public int Id { get; set; }
    public string FullName { get; set; }
    public string City { get; set; }
    public ICollection<Book> Books { get; set; }

  }
}

```


#### Then Create Book ViewModel Class 
##### Properties must also be the class name of the model 
```csharp
using System;

namespace HaniaMapperSample
{
  public  class BookViewModel
  {

    public int Id { get; set; }
    public string Title { get; set; }
    public string Discreption { get; set; }
    public DateTime RegisterDate { get; set; }
    public int AuthorId { get; set; }

    public string AuthorFullName { get; set; }
  }
}

```



#### Then Create BookMapper Class

```csharp
using HaniaMapper.Mapper;
using System;

namespace HaniaMapperSample.Mapper
{
  public class BookMapper : MapperConfig<Book, BookViewModel>
  {

    public override Action<AmdMapperOption<Book, BookViewModel>> CreateConfig()
    {
      return config =>
      {

        //HaniaMapper Map Properties that have the same name as the model class
        //For Extra Property You Can Use Bind Method In Config Action

        config.Bind(x => x.AuthorFullName, c => { c.Map(v => v.Author.FullName); });

      };
    }
  }
}

```
#### Then Create Sample nContext Class For Test Data
```csharp
using System;

namespace HaniaMapperSample.DateContext
{
  public class BookContext
  {
    public Book Book()
    {
      return new Book
      {
        Discreption = "Descreption Of Book",
        Title = "Book Title",
        RegisterDate = DateTime.Now,
        Id = 1,
        AuthorId = 1,
        Author = new Author
        {
          Id = 1,
          City = "Urmia",
          FullName = "Akbar Ahmadi Saray"
        }
      };
    }
  }
}

```

#### You Can Create New Object From BookMapper and Use it
```csharp
using HaniaMapperSample.DateContext;
using HaniaMapperSample.Mapper;

namespace HaniaMapperSample
{
  class Program
  {
    static void Main(string[] args)
    {
      BookContext _dbContext = new BookContext();
      BookMapper mapper = new BookMapper(); 

      Book book = _dbContext.Book();
      BookViewModel bookViewModel = mapper.Map(book);

      Console.WriteLine("------------------------Model------------------------");
      Console.WriteLine(book.Title);
      Console.WriteLine(book.Author.FullName);
      Console.WriteLine("----------------------ViewModel-----------------------");
      Console.WriteLine(bookViewModel.Title);
      Console.WriteLine(bookViewModel.AuthorFullName);
      Console.ReadKey();

      // Go to http://aka.ms/dotnet-get-started-console to continue learning how to build a console app! 
    }
  }
}

      
```

Result On Console
```
------------------------Model------------------------
Book Title
Akbar Ahmadi Saray
----------------------ViewModel-----------------------
Book Title
Akbar Ahmadi Saray

```
Enjoy It :))))


