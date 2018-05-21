# Añadir paquetes externos

Una de las ventajas de utilizar un ecosistema maduro como .NET es que la cantidad de paquetes de terceros y plugins es enorme. Al igual que otros administradores de paquetes, puedes descargar e instalar paquetes .NET que te ayudaran con prácticamente cualquier tarea o problema que puedas imaginar.

NuGet es a su vez el administrador de paquetes y el repositorio oficial de paquetes \([https://www.nuget.org](https://www.nuget.org)\). Puedes buscar paquetes NuGet en internet y posteriormente instalarlos en tu propio equipo a través del terminal \(o mediante la GUI, si utilizas Visual Studio\).

## Instalar el paquete Humanizer

Al final del ultimo capitulo, la aplicación to-do mostrara los items tal que así;

![Fechas en formato ISO 8601](.gitbook/assets/iso8601%20%281%29.png)

La columna de la fecha muestra las fechas en formato que es entendible para las maquinas \(denominado ISO 8601\), pero difícil para los humanos. No seria mucho mejor si fueramos capaces de simplemente leer  "X días desde hoy"?

Podrías escribir tu mismo el código que convierta el formato ISO 8601 en una cadena de texto entendible por el humano, pero afortunadamente existe una solución mas rapida.

El paquete Humanizer de NuGet soluciona este problema facilitándonos métodos que son capaces de "humanizar" o re-escribir prácticamente cualquier cosa: fechas, horas, duraciones, números y un largo etc. Es un proyecto de código abierto realmente fantástico y util que esta publicado bajo la permisiva licencia MIT.

Para añadirlo a tu proyecto, ejecuta este comando en el terminal:

```text
dotnet add package Humanizer
```

Si investigas un poco dentro del fichero del proyecto`AspNetCoreTodo.csproj`, podrás ver una nueva linea `PackageReference` que hace referencia al paquete `Humanizer`.

## Utilizar Humanizer en la vista

Para hacer uso del paquete en tu código, normalmente necesitas añadir la declaración `using` que importa el paquete en la parte superior del fichero.

Dado que el paquete Humanizer sera utilizado para re-escribir las fechas mostradas en la vista, necesitaras hacer uso del mismo en la propia vista. Primero, añade la declaración `@using` en la parte superior de la vista:

`Views/Todo/Index.cshtml`

```markup
@model TodoViewModel
@using Humanizer

// ...
```

A continuación, actualiza la linea encargada de escribir la propiedad `DueAt` para hacer uso del método `Humanize`:

```markup
<td>@item.DueAt.Humanize()</td>
```

Ahora las fechas ya son mucho mas legibles:

![Human-readable dates](.gitbook/assets/friendly-dates%20%281%29.png)

Existen una gran variedad de paquetes disponibles en NuGet para prácticamente todo, desde parsear un fichero XML hasta utilizar el machine learning para postear en Twitter. Al fin y al cabo ASP.NET Core en si mismo no es mas que una colección de paquetes NuGet que son añadidos a tu proyecto.

> El proyecto creando con el comando `dotnet new mvc` incluye una referencia al paquete`Microsoft.AspNetCore.All`, el cual es precisamente un "metapaquete" que hace referencia a todos los otros paquetes que ASP.NET necesita para el proyecto. De esta forma, tu no tienes que tener cientos de referencias a paquetes en tu fichero de proyecto.

En el proximo capitulo, utilizaras otro conjunto de paquetes NuGet \(un sistema denominado Entity Framework Core\) para escribir el código que se comunicará con la base de datos.

