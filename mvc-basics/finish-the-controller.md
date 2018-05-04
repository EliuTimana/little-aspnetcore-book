# Completa el Controlador

## Completa el Controlador

El ultimo paso es completar definitivamente el controlador ToDo. En este momento el controlador contiene una lista de items obtenidos a través del servicio que creamos previamente, y necesitamos enviar dichos items a la vista del modelo `TodoViewModel` para en ultimo lugar unir el modelo con las vista que acabamos de crear:

`Controllers/TodoController.cs`

```csharp
public async Task<IActionResult> Index()
{
    var items = await _todoItemService.GetIncompleteItemsAsync();

    var model = new TodoViewModel()
    {
        Items = items
    };

    return View(model);
}
```

Si aun no lo has hecho, asegúrate de incluir en la parte superior del fichero las siguientes declaraciones  `using`:

```csharp
using AspNetCoreTodo.Services;
using AspNetCoreTodo.Models;
```

Si estas utilizando Visual Studio o Visual Studio Code, el propio editor te sugerirá añadir estas declaraciones cuando pases el cursor sobre las lineas rojas de error.

## Ahora de probar!

Para iniciar la aplicación, presiona F5 \(si estas utilizando Visual Studio o Visual Studio Code\), o simplemente ejecutar el comando `dotnet run` dentro del terminal. Si el código compila sin errores, el servidor arrancara en el puerto 5000 por defecto.

Si tu navegador web no se abre de forma automática, ábrelo tu mismo y dirígete a la siguiente dirección [http://localhost:5000/todo](http://localhost:5000/todo). Deberías ver la vista con la tabla que has creado previamente, con los datos obtenidos de la falsa base de datos \(de forma temporal\).

Enhorabuena! Has creado y ejecutado correctamente tu primera aplicación ASP.NET Core . En el proximo capitulo, seras capaz de ir un paso mas allá con el uso de paquetes de terceros y una base de datos real.

