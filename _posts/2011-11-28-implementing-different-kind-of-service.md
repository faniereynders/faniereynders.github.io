---
layout: post
title: Implementing a different kind of Service Pattern
date: '2011-11-28T16:10:00.000+02:00'
author: Fanie Reynders
tags:
- domain driven design
- design patterns
- object orientation
- test driven design
modified_time: '2012-11-19T16:30:03.954+02:00'
thumbnail: http://3.bp.blogspot.com/-Upq8Zp7FsmM/TtPj3_NzfWI/AAAAAAAAAKQ/V5EDCYhFv4I/s72-c/Drawing5.jpg
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-8117253230268397111
blogger_orig_url: http://www.faniereynders.com/2011/11/implementing-different-kind-of-service.html
---

For the past few months I've investigated many different types of patterns I can implement in my projects; like Repository Patterns, Unit of Work Patterns, Service Patterns etc. After plenty of research, late nights and a gazillion units of black coffee; I finally came across a some sort of hybrid solution that works for me in many different scenarios.

<!--more-->

Being aware that a specific pattern to follow is purely dependant on the particular solution, I wanted to be comfortable with a rock solid "generic" implementation that can be used across many types of applications like [ASP.NET MVC](http://asp.net/mvc), [WPF](http://msdn.microsoft.com/en-us/library/ms754130.aspx), [Silverlight](http://silverlight.net) & [WindowsPhone](http://www.windowsphone.com).

The main reason for this was to make the code more unit testable using mock-ups, this can be implemented easier because everything is very loosely coupled and is utilising the beauty of [dependency injection (DI)](http://en.wikipedia.org/wiki/Dependency_injection).

I want to mention that the following "pattern", if you will; is only a suggestion to those who feel daring and in need for something fresh & different. Your comments are more than welcome.

###What we'll build
<a href="http://3.bp.blogspot.com/-Upq8Zp7FsmM/TtPj3_NzfWI/AAAAAAAAAKQ/V5EDCYhFv4I/s1600/Drawing5.jpg" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="606" src="http://3.bp.blogspot.com/-Upq8Zp7FsmM/TtPj3_NzfWI/AAAAAAAAAKQ/V5EDCYhFv4I/s640/Drawing5.jpg" width="640" /></a>

Okay, don't get bored so soon! Let me walk you through the whole thing. This is a typical ASP.NET MVC implementation, I haven't tried using this in other frameworks yet but I'm very confident that it should work just as well.
	
Scanning the diagram from the bottom, you'll first notice the database. This can be any source of data (SQL, XML files etc.)
	
Next, there's two `IDataContext` implementations: `MockDataContex` and `DataContext`. This serves as the data access layer to our store and can be anything from <a href="http://www.asp.net/entity-framework/tutorials" target="_blank">EntityFramework</a>, <a href="http://nhforge.org/wikis/howtonh/your-first-nhibernate-based-application.aspx" target="_blank">NHibernate</a>&nbsp;etc. Note that the `MockDataContext` does not have any connections to a data store as this will be fabricating mock data for our unit tests.

```csharp
public interface IDataContext
{
	IQueryable<T> All<T>() where T : class;
    IQueryable<T> Find<T>(Expression<Func<T, bool>> predicate) where T : class;
    T GetOne<T>(Expression<Func<T, bool>> predicate) where T : class;
    T Add<T>(T entity) where T : class;
    T Update<T>(T entity) where T : class;
    T Remove<T>(T entity) where T : class;
    bool SaveChanges();
}
```
	
By implementing the <a href="http://en.wikipedia.org/wiki/Factory_method_pattern" target="_blank">Factory Pattern</a>, it allows us to create an disposable instance of different `IDataContext` implementations, in turn better managing database connections & utilising connection pooling.
	
The only responsibility of the `IDataContextFactory` implementation, is to create the applicable instance of the data contexts to be consumed.

```csharp
public interface IDataContextFactory
{
    IDataContext Context { get; }   
}

[Export(typeof(IDataContextFactory))]
public class DataContextFactory : IDataContextFactory
{
    public IDataContext Context
    {
        get { return new DataContext(); }
    }
}
```
    
We all may be familiar with the next item, the Service; that houses all the business logic of each unit of work. The `InvoiceService` implements `IInvoiceService` which forces an property of type `IDataContextFactory` named "factory". 

```csharp
public interface IInvoiceService
{
    IDataContextFactory factory { get; set; }
    Invoice GetInvoice(string invoiceNumber);
    bool SaveNewInvoice(Invoice newInvoice);
}

[Export(typeof(IInvoiceService))]
public class InvoiceService: IInvoiceService
{
    [Import]
    private IDataContextFactory _factory;
    public IDataContextFactory factory
    {
        get { return _factory; }
        set { _factory = value; }
    }
    
    public Invoice GetInvoice(string invoiceNumber)
    {
        using (var context = factory.Context)
        {
            var invoice = context.Find<Invoice>(i => i.InvoiceNumber.Equals(invoiceNumber,  StringComparison.OrdinalIgnoreCase));
            return invoice;
        }
    }
    
    public bool SaveNewInvoice(Invoice newInvoice)
    {
        using (var context = factory.Context)
        {
            var invoice = context.Add<Invoice>(newInvoice);
            context.SaveChanges();
            return invoice != null;
        }
    }
}
```
    
All the classes mentioned above, lives in the Domain layer of the solution. The next few items is applicable for a ASP.NET MVC implementation:

I'm not going into detail of MVC, but the general rule of thumb is to *keep your views skinny, have thin controllers and fat models* (View Models). It's also *best practise to have a ViewModel for every View* so that unit testing is possible on your view logic. Keep the behavior logic in the Controllers, view logic in the ViewModels &amp; business logic in the Domain.

Moving on, The `InvoiceController` consumes a service of type `IInvoiceService`. (Not to be confused by an web & WCF service, the term "service" here merely acts as a host of business specific tasks exposed by the contract `IInvoiceService`

The Controller contains many Actions that is responsible for relaying commands, DomainModel-ViewModel mappings & showing certain Views depending on request. In this case, an Action in the `InvoiceController` can decide to pass the ViewModel to either a Desktop or Mobile View.

The Controller's Action method may use the helper class `InvoiceComposer` to map from Domain Models to ViewModels and back. There are many Object Mappers available like <a href="http://automapper.codeplex.com/" target="_blank">AutoMapper</a>.

```csharp
public class InvoiceController : Controller
{
    private readonly IInvoiceService invoiceService;
    public InvoiceController(IInvoiceService invoiceService)
    {
        this.invoiceService = invoiceService;
    }
    
    public ActionResult Details(string invoiceNumber)
    {
        var domainModel = invoiceService.GetInvoice(invoiceNumber);
        var viewModel = domainModel.ToViewModel();
        if (Request.Browser.IsMobileDevice)
        {
            return View("Invoice.Mobile", viewModel);
        }
        return View(viewModel);
    }
    
    [HttpPost]
    public ActionResult Save(InvoiceViewModel viewModel)
    {
        var domainModel = viewModel.ToDomainModel();
        invoiceService.SaveNewInvoice(domainModel);
        return View(viewModel);
    }
}
```
    
###Wiring everything together
By utilising the beauty of dependency injection, we can create a container that houses the configuration mappings of contracts to the implementation classes. In return, we just expect the different&nbsp;types concerned to be magically instantiated for us by the Dependency Resolver. No ugly *"Object reference not set to an instance of an object"* exceptions (or at least if the DI is setup correctly).

In this example, I'll be using <a href="http://mef.codeplex.com/" target="_blank">MEF</a> (no not the drug) for the DI framework. MEF is obviously short for <a href="http://mef.codeplex.com/" target="_blank">Managed Extensible Framework</a> and is way more than just an Dependency Injection engine.

> Note the `[Import]` & `[Export]` attributes on the code above. This tells MEF to Export the implementation class as type of the contract specified and imports the instance of the implementation class when required.

<a href="http://msdn.microsoft.com/en-us/library/dd460648.aspx" target="_blank">Here is some cool resources on MEF for anyone that's interested</a>

I don't want to steal <a href="http://www.youtube.com/watch?v=hvlHi7iTdaw" target="_blank">Steve Job's line</a> here but... *There is one more thing...* 

###Unit Testing
Having our code so loosely coupled means better unit testing which means more rock solid systems which means happier clients which means more money :)

Okay but its not all about the money, it's about that air punch at the end of the day when you feel you just created the best and cleanest system in the world.

Going into no detail here is what a typical unit test may look like with mocking in place:

```csharp
[TestMethod()]
public void GetInvoiceTest()
{
    var invoiceService = new InvoiceService();
    //This MockDataContextFactory also implements IDataContextFactory
    invoiceService.factory = new MockDataContextFactory();
    string invoiceNumber = "INA001";
    var expected = new Invoice { 
        Id = 1, 
        InvoiceNumber = "INA001", 
        Amount = 100 };
    var actual = invoiceService.GetInvoice(invoiceNumber);
    Assert.IsInstanceOfType(actual, typeof(Invoice));
    Assert.AreEqual(expected, actual);
}
```
    
So there you go. Another mouth-full from me.

I hope that this may help everyone else interested. Like I said, I urge my fellow Ninjas to please feel free to comment, correct my possible wrongs or even elaborate on a possible other solution. This works perfectly for me.

*Till next time...* Cheers.

<a href="http://twitter.com/faniereynders" target="_blank">Follow and Tweet with me on Twitter: @FanieReynders</a>