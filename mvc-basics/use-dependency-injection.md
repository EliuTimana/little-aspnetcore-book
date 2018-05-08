# Utiliza la Inyección de Código

Volviendo al controlador `TodoController`, añade las siguientes lineas de código para poder trabajar con la interfaz `ITodoItemService`:

```csharp
public class TodoController : Controller
{
    private readonly ITodoItemService _todoItemService;

    public TodoController(ITodoItemService todoItemService)
    {
        _todoItemService = todoItemService;
    }

    public IActionResult Index()
    {
        // Obtener los items de la base de dastos

        // Convertir los items al modelo

        // Pasar la vista al modelo y renderizarla
    }
}
```

Dado que `ITodoItemService` pertenece al namespace `Services`, necesitaras importar la declaración `using` al comienzo del fichero:

```csharp
using AspNetCoreTodo.Services;
```

La primera linea de la clase declara una variable privada para obtener un objeto de la clase `ITodoItemService`. Esta variable nos permitirá posteriormente utilizar el servicio dentro de la acción/método `Index`  \(lo verás en unos instantes\).

La linea `public TodoController(ITodoItemService todoItemService)` define el **constructor** de la clase. El constructor es un método de tipo especial el cual es llamado cada vez que necesites crear una instancia de la su propia clase \(la clase `TodoController` en este caso en particular\). Añadiendo al constructor el parámetro `ITodoItemService`, estas indicando a tu aplicación que para poder crear una instancia de la clase `TodoController`, necesitas que la propia aplicación te proporcione un objeto del tipo de la interfaz `ITodoItemService`.

> Las interfaces son realmente útiles ya que permiten desacoplar \(separar\) la lógica de tu aplicación. Dado que el controlador depende en la interfaz`ITodoItemService`, y no de una _clase en concreto_, la aplicación no debe preocuparse de que clase en realidad esta implementando la interfaz. Podría ser la clase `FakeTodoItemService`, una nueva clase que se conectaría a otro tipo de base de datos, o muchas opciones más! Siempre y cuando la clase implemente correctamente la interfaz, el controlador hará usa de ella. Esto hace realmente sencillo testear partes de tu aplicación por separado. Este aspecto sera cubierto en detalle dentro del capitulo _Pruebas Automatizadas_.

Ahora ya estas en condiciones de utilizar la interfaz `ITodoItemService` \(a través de la variable privada que has declarado previamente\) dentro de la método que obtiene el conjunto de items mediante la capa de servicio:

```csharp
public IActionResult Index()
{
    var items = await _todoItemService.GetIncompleteItemsAsync();

    // ...
}
```

Recuerdas que el método`GetIncompleteItemsAsync` devuelve un objeto de tipo `Task<TodoItem[]>`? Devolver un objeto de tipo`Task` significa que el método no tiene porque tener necesariamente el resultado inmediatamente, por ello puedes hacer uso de la palabra clave `await` asegurándote que tu código espera hasta que el resultado esta listo antes de continuar con la ejecución.

El uso de objetos de tipo `Task` es comúnmente utilizado cuando es necesario acceder a una base de datos o hacer una petición a una API, dado que el el resultado puede no estar disponible hasta que la base de datos o la red estén disponibles. Si has utilizado promesas o callbacks en JavaScript o en otros lenguajes, `Task` se basa en la misma idea: la promesa se convertirá en un resultado en algún momento del futuro.

> Si has tenido que sufrir con el denominado  "callback hell" en código JavaScript, estas de suerte. Trabajar con código asíncrono en .NET es mucho mas sencillo gracias a la palabra clave`await`. `await` permite pausar tu código en un operación asíncrona, y continuar donde se quedo pausada cuando la respuesta de la base de datoe. In the meantime, your application isn't blocked, because it can process other requests as needed. This pattern is simple but takes a little getting used to, so don't worry if this doesn't make sense right away. Just keep following along!

The only catch is that you need to update the `Index` method signature to return a `Task<IActionResult>` instead of just `IActionResult`, and mark it as `async`:

```csharp
public async Task<IActionResult> Index()
{
    var items = await _todoItemService.GetIncompleteItemsAsync();

    // Put items into a model

    // Pass the view to a model and render
}
```

You're almost there! You've made the `TodoController` depend on the `ITodoItemService` interface, but you haven't yet told ASP.NET Core that you want the `FakeTodoItemService` to be the actual service that's used under the hood. It might seem obvious right now since you only have one class that implements `ITodoItemService`, but later you'll have multiple classes that implement the same interface, so being explicit is necessary.

Declaring \(or "wiring up"\) which concrete class to use for each interface is done in the `ConfigureServices` method of the `Startup` class. Right now, it looks something like this:

**Startup.cs**

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // (... some code)

    services.AddMvc();
}
```

The job of the `ConfigureServices` method is adding things to the **service container**, or the collection of services that ASP.NET Core knows about. The `services.AddMvc` line adds the services that the internal ASP.NET Core systems need \(as an experiment, try commenting out this line\). Any other services you want to use in your application must be added to the service container here in `ConfigureServices`.

Add the following line anywhere inside the `ConfigureServices` method:

```csharp
services.AddSingleton<ITodoItemService, FakeTodoItemService>();
```

This line tells ASP.NET Core to use the `FakeTodoItemService` whenever the `ITodoItemService` interface is requested in a constructor \(or anywhere else\).

`AddSingleton` adds your service to the service container as a **singleton**. This means that only one copy of the `FakeTodoItemService` is created, and it's reused whenever the service is requested. Later, when you write a different service class that talks to a database, you'll use a different approach \(called **scoped**\) instead. I'll explain why in the _Use a database_ chapter.

That's it! When a request comes in and is routed to the `TodoController`, ASP.NET Core will look at the available services and automatically supply the `FakeTodoItemService` when the controller asks for an `ITodoItemService`. Because the services are "injected" from the service container, this pattern is called **dependency injection**.

