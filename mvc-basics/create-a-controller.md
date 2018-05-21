# Crea un Controlador

Actualmente ya existe una serie de controlador en el directorio del proyecto, incluyendo `HomeController` encargado de renderizar la pagina de bienvenida por defecto que se muestra cuando visitas la dirección`http://localhost:5000`. Puedes ignorar estos controladores por ahora.

Crear un nuevo controlador, llámalo`TodoController`, encargado de la funcionalidad de la aplicación To-Do y añade el siguiente código:

`Controllers/TodoController.cs`

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;

namespace AspNetCoreTodo.Controllers
{
    public class TodoController : Controller
    {
        // Las acciones iran aquí
    }
}
```

Las Rutas que maneja el controlador son denominadas **acciones** y estas están representadas por métodos dentro de la clase del controlador. Por ejemplo, el controlador  `HomeController` incluye 3 acciones \(métodos\) \(`Index`, `About`y `Contact`\) los cuales son mapeados por ASP.NET Core a las siguientes rutas URL:

```text
localhost:5000/Home         -> Index()
localhost:5000/Home/About   -> About()
localhost:5000/Home/Contact -> Contact()
```

Existen una serie de convenciones \(patrones comunes\) utilizados por ASP.NET Core, como es el caso de `FooController` el cual responde a la URL `/Foo`y la acción `Index` puede quedar fuera de la dirección URL ya que es la ejecutada por defecto. Este comportamiento es modificable si lo crees necesario, pero por ahora vamos a seguir utilizando los convencionalismos.

Añade una nueva acción denominada `Index` en el controlador`TodoController`, modificando el comentario `// Las acciones iran aqui`:

```csharp
public class TodoController : Controller
{
    public IActionResult Index()
    {
        // Obtiene los items de la base de datos

        // Convierte los items en modelos

        // Renderiza la vista utilizando el modelo
    }
}
```

Los métodos \(acciones\) pueden retornar tanto vistas, como datos en formato JSON, o estados HTTP como puede ser el caso de `200 OK` y `404 Not Found`. El tipo que devuelve`IActionResult` permite retornar cualquiera de esos tipos anteriormente citados.

  
Una muy buena practica es mantener los controladores los mas simples posibles. En este caso, el controlador sera el responsable de obtener los items de la base de datos, generar un modelo con dichos items que la vista puede comprender y por ultimo enviar una vista de vuelta al navegador del usuario.

Antes de poder completar el resto del controlador es necesario crear un modelos y una vista.

