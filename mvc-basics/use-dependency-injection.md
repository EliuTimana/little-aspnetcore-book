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

Ahora ya estas en condiciones de utilizar la interfaz `ITodoItemService` \(a través de la variable privada que has declarado previamente\) dentro del método que obtiene el conjunto de items mediante la capa de servicio:

```csharp
public IActionResult Index()
{
    var items = await _todoItemService.GetIncompleteItemsAsync();

    // ...
}
```

Recuerdas que el método`GetIncompleteItemsAsync` devuelve un objeto de tipo `Task<TodoItem[]>`? Devolver un objeto de tipo`Task` significa que el método no tiene porque tener necesariamente el resultado inmediatamente, por ello puedes hacer uso de la palabra clave `await` asegurándote que tu código espera hasta que el resultado esta listo antes de continuar con la ejecución.

El uso de objetos de tipo `Task` es comúnmente utilizado cuando es necesario acceder a una base de datos o hacer una petición a una API, dado que el el resultado puede no estar disponible hasta que la base de datos o la red estén disponibles. Si has utilizado promesas o callbacks en JavaScript o en otros lenguajes, `Task` se basa en la misma idea: la promesa se convertirá en un resultado en algún momento del futuro.

> Si has tenido que sufrir con el denominado  "callback hell" en código JavaScript, estas de suerte. Trabajar con código asíncrono en .NET es mucho mas sencillo gracias a la palabra clave`await`. `await` permite pausar tu código en una operación asíncrona, y continuar donde se quedo pausada cuando la respuesta de la base de datos este lista. Durante el proceso de resolución de la promesa tu aplicación no quedara bloqueada, porque podrá seguir procesando otras peticiones si es necesario. Este patrón de funcionamiento es realmente simple pero cuesta un poco hacerse a él, así que no te preocupes si ahora mismo no lo acabas de comprender por  completo. Simplemente sigue adelante con el tutorial!

La única modificación que tienes que hacer en el código se encuentro en el método `Index` para que devuelve un objeto de tipo `Task<IActionResult>` en lugar de u`IActionResult`, y por supuesto marcar el método como `async`:

```csharp
public async Task<IActionResult> Index()
{
    var items = await _todoItemService.GetIncompleteItemsAsync();

    // Convertir los items al modelo

    // Pasar la vista al modelo y renderizarla
}
```

Ya casi lo has logrado! Has modificado el controlador `TodoController` para que dependa de la interfaz `ITodoItemService`, pero no le has especificado a tu aplicación ASP.NET Core cual el servicio que implementa la interfaz y es utilizado por debajo. Puede parecer obvio ahora mismo dado que solo tienes un clase que implemente la interfaz  `ITodoItemService`, pero mas adelante dispondrás de varias clases que implementaran todas la misma interfaz, por lo que es necesario especificar cual en concreto se debe utilizar.

La declaración de que clase en concreto sera la utilizada para cada interfaz se realiza en el método`ConfigureServices` de la clase`Startup`. Ahora mismo debería tener un aspecto tal que así:

**Startup.cs**

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // (...)

    services.AddMvc();
}
```

El trabajo del método `ConfigureServices` es añadir elementos al **contenedor de servicio**, o la colección de servicios que la aplicación ASP.NET Core tiene constancia. La linea con el codigo`services.AddMvc()` añade los servicios que internamente necesita nuestra aplicación \(puedes probar a comentar esta linea y ver que sucede\). Cualquier otro servicio que necesites utilizar en tu aplicación debe ser añadido al contenedor de servicios de igual forma dentro de él método `ConfigureServices`.

Añade la siguiente línea dentro del método `ConfigureServices` :

```csharp
services.AddSingleton<ITodoItemService, FakeTodoItemService>();
```

Esta linea le dice a la aplicación ASP.NET que debe utilizar la clase`FakeTodoItemService` allá donde la interfaz `ITodoItemService` sea requerida dentro de un constructor \(o en cualquier otro lugar\).

`AddSingleton` añade el servicio requerido previamente al contenedor de servicios como **singleton**. Esto significa que solo se creara una copia para toda la aplicación del servicio`FakeTodoItemService`, y que esta sera re-utilizada cuando el servicio sea requerido. Más adelante, cuando hayamos creado otra clase de servicio que interactue con la base de datos, verás como se hará uso de un planteamiento diferente \(denominado **scoped**\). Esta parte se explica con mas detalle dentro del capitulo _Uso de una base de datos_.

Y esto es todo por el momento! Cuando una petición llega a la aplicación y es enviada al controlador`TodoController`, ASP.NET Core buscara entre todos los servicios disponibles y automáticamente suministrara el serivcio`FakeTodoItemService` cuando el controlador pregunte por la interfaz`ITodoItemService`. Debido a que los servicios son "inyectados" desde el contenedor de servicios, esta patrón de funcionamiento es denominado **inyección de dependencia**.

