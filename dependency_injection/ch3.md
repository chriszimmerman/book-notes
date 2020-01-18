# Chapter 3 - Writing Loosely Coupled Code

When we write software, we prefer to start in the most significant place — the part that has most visibility to our stakeholders. As in Mary’s e-commerce application, this is often the UI. From there, we work our way in, adding more functionality until one feature is done; then we move on to the next. This outside-in technique helps us to focus on the requested functionality without overengineering the solution.

In chapter 2, Mary used the opposite approach. She started with the data access layer and worked her way out, working inside-out. It would be harsh for us to say that working inside-out is bad, but as you’ll see later, the outside-in approach gives you quicker feedback on what you’re building. We’ll therefore build the application in the opposite order, starting with the UI layer, continuing with the domain layer, and then building the data access layer last.

The outside-in technique is closely related to the YAGNI principle — “You Aren’t Gonna Need It.” This principle emphasizes that only required features should be implemented, and that the implementation should be as simple as possible.

A Data Transfer Object (DTO) is an object that carries data between processes.

Recall from the analysis of Mary’s implementation that depending on Volatile Dependencies is a cardinal sin. As soon as you do that, you’re tightly coupled with the type just used. To avoid this tight coupling, we’ll introduce an interface and use a technique called Constructor Injection; how the instance is created, and by whom, is of no concern to HomeController.

```
public class HomeController : Controller
{
    private readonly IProductService productService;

    public HomeController(
        IProductService productService)
    {
        if (productService == null)
            throw new ArgumentNullException(
                "productService");

        this.productService = productService;
    }

    public ViewResult Index()
    {
        IEnumerable<DiscountedProduct> products =
            this.productService.GetFeaturedProducts();

        var vm = new FeaturedProductsViewModel(
            from product in products
            select new ProductViewModel(product));

        return this.View(vm);
    }
}
```

The first time we heard about Constructor Injection, we had a hard time understanding the real benefit. Doesn’t it push the burden of controlling the Dependency onto some other class? Yes, it does — and that’s the whole point. In an n-layer application, you can push that burden all the way to the top of the application into a Composition Root.

## Composition Root

As we discussed in section 1.4.1, we’d like to be able to compose our classes into applications in a way similar to how we plug electrical appliances together. This level of modularity can be achieved by centralizing the creation of our classes into a single place. We call this location the Composition Root.

The Composition Root is located as close as possible to the application’s entry point. In most .NET Core application types, the entry point is the Main method. Inside the Composition Root, you can decide to compose your application manually — that’s using Pure DI — or to delegate it to a DI Container.

--

The principle of programming to interfaces instead of concrete classes is a cornerstone of DI. It’s this principle that lets you replace one concrete implementation with another. Before continuing, we should take a quick moment to recognize the role of interfaces in this discussion.

Programming to interfaces doesn’t mean that all classes should implement an interface. It typically makes little sense to hide POCOs, DTOs, and view models behind an interface, because they contain no behavior that requires mocking, Interception, or replacement. Because DiscountedProduct, FeaturedProductsViewModel, and ProductViewModel are (view) models, they implement no interface. We’ll take another look at whether to use interfaces or abstract classes later in this section.

--

## Entity

An Entity is a term from Domain-Driven Design that covers a domain object that has a long-term identity unrelated to a particular object instance.7  This may sound abstract and theoretical, but it means that an Entity represents an object that lives beyond arbitrary bits in memory. Any .NET object instance has an in-memory address (identity), but an Entity has an identity that lives across process lifetimes.

We often use databases and primary keys to identify Entities and ensure that we can persist and read them even if the host computer reboots. The domain object Product is an Entity because the concept of a product has a longer lifetime than a single process, and we use a product ID to identify it in IProductRepository.

--

Because our goal is to invert the dependency between the domain layer and the data access layer, IProductRepository is defined in the domain layer. In the next section, we’ll create an implementation of IProductRepository as part of the data access layer. This allows our dependency to point at the domain layer.

**By letting ProductService depend on IProductRepository, we allow behavior to be replaced or Intercepted. By placing that behavior in a different library, we allow a whole library to be replaced.**

Is partway through 3.1.2 - building an independent domain model