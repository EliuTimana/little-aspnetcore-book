# Añade un Servicio

Ya has creado un modelo, una vista y un controlador. Previamente has utilizado el modelo y la vista dentro del controlador, ademas has necesitado escribir el código encargado de obtener los items de tipo To-Do de la base de datos.

Es posible escribir el código de acceso a la base de datos en el propio controlador, pero una mejor practica es mantener el código lo mas separado posible. Por qué? En una aplicación grande, como podría ser una aplicación real de tamaño importante, necesitaras hacer uso de mucho conceptos como pueden ser:

* **Renderizado de vistas** y manejo de datos de entrada: este el trabajo principal del controlador.
* **Desarrollar la lógica de tu negocio**, o código que esta relacionado con el fin ultimo del propósito y "negocio" de tu aplicación. En una aplicación de tipo To-Do, la lógica del negocio se referirá a las decisiones que se deben tomar respecto a aspectos como el valor de la fecha para nuevos items o si se decide solo mostrar las tareas que están incompletas. Otros ejemplos de lógica de negocio incluyen el calculo del precio total de un articulo a partir de su coste y sus impuestos o comprobar si un jugador tienes los puntos suficientes para subir de nivel dentro del juego.
* **Guardar y obtener** items de la base de datos.

Nuevamente, es posible realizar todos estos puntos en un solo u enorme controlador, pero rápidamente se convertiría en un controlador complicado de manejar y validar. Además, es ,muy común hoy en día ver las aplicaciones divididas en una, dos, tres o mas capas o _tiers_, cada una de ellas encargada de manejar uno de los puntos anteriormente comentados. Esto ayuda a mantener el controlador lo mas simple posible, y permite validarlo de una manera mucho mas sencilla , también nos permitirá cambiar la lógica del negocio y el código de acceso a la base de datos de una forma mas sencilla.

Separar tus aplicación según este patron suele ser denominado **multi-tier** o **arquitectura n-tier**. En algunos casos, las capas \(tiers\) están completamente aisladas unas de otras en proyectos distintos, sim embargo otras veces simplemente se refiere a como están utilizadas y organizadas las clases de la aplicación. El punto clave aquí es pensar como podemos dividir nuestra aplicación en pequeñas piezas manejables de forma individual, y de esta forma evitamos tener controladores o clases gigantescos que intentan de ocuparse todo.

Para este proyecto, vamos a utilizar 2 capas a nivel de aplicación: una **capa de presentación **formada por las vistas y los controladores que interactúan con el usuario y una **capa de servicio** que contendrá la lógica del negocio y el código encargado del acceso a la base de datos. En realidad la capa de presentación ya existe, por lo que el siguiente paso consistirá en crear un servicio que se encargue de la lógica del negocio del la aplicación To-Do y de salvar los items de tipo To-Do en la base de datos.

> Proyectos de mayor tamaño suelen utilizar una arquitectura de tipo 3-tier: una capa de presentación, una capa de servicio y una capa de datos. Una **capa datos** es una clase que se centra exclusivamente en el acceso a la base de datos \(y no en la lógica de negocio\). En nuestra aplicación, vamos a combinar estas 2 ultimas capas en un única y sencilla capa de servicio, pero siéntete completamente libre de experimentar con diferentes tipos de arquitecturas para organizar este código.

## Crea una interfaz

C\# introduce el concepto de **interfaz**, donde la definición de los métodos y propiedades de un objeto esta separada de la clase donde están finalmente ubicados dicho métodos y propiedades. Las interfaces nos facilitan tener nuestras clases independientes, desacopladas y fáciles de testear , como podrás ver a continuación \(y más adelante en el capitulo _Pruebas Automatizadas_\). La interfaz se utilizara para representar el servicio que sera capaz de interactuar con los items de la base de dastos.

Por convención, las interfaces llevan el prefijo "I" en su nombre. Crea un nuevo fichero en el directorio de Servicios:

`Services/ITodoItemService.cs`

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using AspNetCoreTodo.Models;

namespace AspNetCoreTodo.Services
{
    public interface ITodoItemService
    {
        Task<TodoItem[]> GetIncompleteItemsAsync();
    }
}
```

Es importante destacar que el `namespace `de este fichero es` AspNetCoreTodo.Services`. Los Namespaces son una forma de organizar nuestros ficheros .NET, y es una buena practica que el `namespace` coincida con el nombre del directorio donde esta alojado el fichero \(`AspNetCoreTodo.Services` para ficheros en la directorio `Services` \).

Dado que este fichero \(perteneciente al namespace `AspNetCoreTodo.Services`\) hace referencia a la clase `TodoItem`\(perteneciente al namespace `AspNetCoreTodo.Models`\), es necesario incluir la declaración `using` en la parte superior del fichero para incluir este namespace. Sin la inclusion de esta declaración `using`, tendremos un error en nuestro código del siguiente tipo:

```text
The type or namespace name 'TodoItem' could not be found (are you missing a using directive or an assembly reference?)
```

Dado que este fichero es una interfaz no existe apenas código en su interior, simplemente la definición del método `GetIncompleteItemsAsync`. Este método no requiere parámetros y devuelve un objeto de tipo `Task<TodoItem[]>`.

> Si esta sintaxis te resulta un poco confusa, piensa: "una Task que contiene un array de TodoItems".

El tipo `Task` es similar a un promesa o un futuro, y es necesario utilizarlo porque la ejecución de este método va a ser de tipo **asíncrona**. En otras palabras, es posible que el método no sea capaz de devolver una lista de items de tipo to-do directamente porque necesita comunicarse con la base de datos en primer lugar. \(Hablaremos mas acerca de este punto\)

## Crea una clase para el Servicio

Ahora que la interfaz esta definida, necesitas crear la clase realmente encargada del servicio. Cubriré con mayor profundizar el código de la base de datos en el capitulo _Uso de bases de datos,_ así que por ahora vamos a falsear el resultado y retornar siempre dos items escritos directamente en el código:

`Services/FakeTodoItemService.cs`

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using AspNetCoreTodo.Models;

namespace AspNetCoreTodo.Services
{
    public class FakeTodoItemService : ITodoItemService
    {
        public Task<TodoItem[]> GetIncompleteItemsAsync()
        {
            var item1 = new TodoItem
            {
                Title = "Aprende ASP.NET Core",
                DueAt = DateTimeOffset.Now.AddDays(1)
            };

            var item2 = new TodoItem
            {
                Title = "Crea apliaciones increibles",
                DueAt = DateTimeOffset.Now.AddDays(2)
            };

            return Task.FromResult(new[] { item1, item2 });
        }
    }
}
```

Esta clase `FakeTodoItemService` implementa la interfaz `ITodoItemService` pero siempre devuelve el mismo array con dos elementos de tipo `TodoItem`. Más adelante utilizarás esta clase para testar el controlador y la vista, para finalmente añadir código real de acceso a la base de datos en el capítulo _Uso de bases de datos_.

