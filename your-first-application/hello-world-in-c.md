# Hello World en C\#

Antes de profundizar con mayor detalle dentro de ASP.NET Core, vamos a intentar crear y ejecutar una aplicación muy sencilla en C\#.

En primer lugar, abre tu terminal favorito \(o si utilizas Windows será PowerShell\). Navega a la carpeta donde quieras crear y almacenar tu proyectos, como puede ser por ejemplo la carpeta Documents:

```text
cd Documents
```

Haz uso del comando`dotnet` para crear un nuevo proyecto C\#:

```text
dotnet new console -o CsharpHelloWorld
```

El comando`dotnet new` crea un nuevo proyecto .NET, escrito en C\# por defecto. El parametro`console` seleccionar una plantilla para la creación de una aplicación de consola \(un programada que muestra texto por pantalla\). El parámetro`-o CsharpHelloWorld` le dice a `dotnet new` que debe crear un nuevo directorio llamado`CsharpHelloWorld` para almacenar todos los ficheros del proyecto. Muévete al directorio que acabas de crear:

```text
cd CsharpHelloWorld
```

El comando`dotnet new console` crea un programa básico que muestra por pantalla el texto`Hello World!`. El programa esta compuesto por 2 ficheros: el fichero del proyecto \(con extensión `.csproj`\) y una fichero con código C\# \(con extensión `.cs`\). Si abres dichos ficheros en tu editor de texto, veras algo tal que así:

`CsharpHelloWorld.csproj`

```markup
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

</Project>
```

El fichero del proyecto sigue el standard XML y define una serie de propiedades y metadata acerca del proyecto. Mas adelante, cuando haga referencia a otros paquetes, estos se listaran aquí \(similar a como funciona el fichero `package.json` de npm\). Afortunadamente no tendrás que editar este fichero a mano muy a menudo.

`Program.cs`

```csharp
using System;

namespace CsharpHelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

`static void Main` es el punto de entrada para todo programa escrito en C\#, y por convención esta siempre ubicado en una clase llamada `Program`. La declaración`using` al comienzo del fichero importa las clases del paquete`System` de .NET y las hace disponibles para todo el código de la clase.

Dentro de la carpeta del proyecto, puedes hacer uso del comando `dotnet run`  para ejecutar el programa. Deberías ver la salida programa escrita en la pantalla después de que el programa haya compilado:

```text
dotnet run

Hello World!
```

Esto es todo lo necesario que necesitas saber para crear y ejecutar un programa .NET! A continuación, seguirás los mismos pasos para crear una aplicación ASP.NET Core.

