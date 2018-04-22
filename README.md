# Introducción

Gracias por elegir El pequeño libro sobre ASP.NET Core! He escrito este pequeño libro con la intención de ayudar tanto programadores como a toda la gente interesada en aprender acerca de ASP.NET Core 2.0, un nuevo framework para construir aplicaciones webs y APIs.

El pequeño libro sobre ASP.NET Core Book esta estructurado como un tutorial. Con él serás capaz de construir una aplicación To-Do de inicio a fin mientras aprendes:

* Los conceptos básicos sobre el MVC \(Modelo-Vista-Controlador\).
* Conocer como el código front-end \(HTML, CSS, JavaScript\) trabaja en conjunto con el código back-end.
* Que es la Inyección de dependencias y porque es tan útil.
* Como leer y escribir datos en una base de datos.
* Como añadir acceso, registro y seguridad a tu aplicación.
* Como publicar una app en internet.

No te preocupes, no necesitas tener conocimientos previos acerca de ASP.NET Core \(o acerca de ninguno de los puntos anteriores\) para comenzar con este libro.

## Antes de comenzar

El resultado final de la aplicación esta disponible en GitHub \([https://www.github.com/nbarbettini/little-aspnetcore-todo](https://www.github.com/nbarbettini/little-aspnetcore-todo)\). Siéntete libre para descargarlo si tienes dudas o necesitas comparar tu propio código.

El libro esta inmerso en una continua actualización con correcciones de errores y nuevo contenido. Si estas leyendo la versión en PDF, eBook, o en papel, visita la pagina oficial \([littleasp.net/book](http://www.littleasp.net/book)\) y comprueba si exista una versión mas actual disponible. En la ultima pagina del libro se encuentra una lista detallada con las versiones y el log de cambios.

### Lectura en otros idiomas

Gracias al gran trabajo de algunos compañeros fantásticos , el pequeño libro ASP.NET Core Book ha sido traducido a otros idiomas:

* [**ASP.NET Core El Kitabı**](https://sahinyanlik.gitbooks.io/kisa-asp-net-core-kitabi/) \(Turkish\)
* [**简明 ASP.NET Core 手册**](https://windsting.github.io/little-aspnetcore-book/book/) \(Chinese\)
* [**The Little ASP .Net Core Book**](https://nbarbettini.gitbooks.io/little-asp-net-core-book) \(English\)

## Para quien esta destinado este libro?

Si eres nuevo en el mundo de la programación, este libro te introducirá los principales patrones y modelos usados actualmente para construir paginas web. Aprenderás como construir una aplicación web \(y como encajan sus principales partes\) partiendo desde cero! La intención de este libro no es cubrir absolutamente todos los conceptos que necesitas saber acerca de la programación, sin embargo te aportara un punto de partida suficiente desde el cual poder aprender conceptos mas avanzados.

Si actualmente ya tienes experiencia con código back-end como Node, Python, Ruby, Go, o Java, muchos de los conceptos del libro te resultaran familiares como MVC, Plantillas de Vistas e Inyección de Código. El código de este libro esta escrito en C\#, pero no debería ser muy diferente en otros lenguajes que ya puedes conocer.

Si por otra parte eres un programador de ASP.NET MVC, te sentirás como en casa! ASP.NET Core añade nuevas herramientas y reutiliza \(y simplifica\) conceptos que ya conoces. A continuación detallare algunas de las principales diferencias.

No importa cual es tu experiencia previa con la programación web, este libro te enseñara todo lo que necesitas saber para crear una simple y util aplicación web en ASP.NET Core. Aprenderás como construir la funcionalidad programando tanto la parte del front-end como la del back.end, como interactuar con una base datos y como realizar tests y publicar la aplicación para todo el mundo.

## Que es ASP.NET Core?

ASP.NET Core es un framework web creado por Microsoft para la construcción de aplicaciones web, APIs, y micro-servicios. Utiliza los principales patrones usados actualmente como MVC \(Modelo-Vista-Controlador\), Inyección de Código, y un Middleware encargado de manipular las peticiones de entrada. El framework es de código abierto con una licencia Apache 2.0, lo que significa que el código esta completamente disponible, y la comunidad esta comprometida a la contribución para la detección de errores y nuevas mejoras.

ASP.NET Core corre encima del tiempo de ejecución de Microsoft's .NET, similar al funcionamiento de la maquina virtual Java \(JVM\) ó el interprete de Ruby. Por tanto puedes desarrollar tu aplicaciones ASP.NET Core en una gran variedad de lenguajes como C\#, Visual Basic ó F\#. C\# es la opción mas utilizada, y por tanto sera la que usaremos en este libro. Puedes construir y ejecutar tus aplicaciones ASP.NET Core tanto en maquinas Windows, como Mac y Linux.

## Why do we need another web framework?

There are a lot of great web frameworks to choose from already: Node/Express, Spring, Ruby on Rails, Django, Laravel, and many more. What advantages does ASP.NET Core have?

* **Speed.** ASP.NET Core is fast. Because .NET code is compiled, it executes much faster than code in interpreted languages like JavaScript or Ruby. ASP.NET Core is also optimized for multithreading and asynchronous tasks. It's common to see a 5-10x speed improvement over code written in Node.js.
* **Ecosystem.** ASP.NET Core may be new, but .NET has been around for a long time. There are thousands of packages available on NuGet \(the .NET package manager; think npm, Ruby gems, or Maven\). There are already packages available for JSON deserialization, database connectors, PDF generation, or almost anything else you can think of.
* **Security.** The team at Microsoft takes security seriously, and ASP.NET Core is built to be secure from the ground up. It handles things like sanitizing input data and preventing cross-site request forgery \(XSRF\) automatically, so you don't have to. You also get the benefit of static typing with the .NET compiler, which is like having a very paranoid linter turned on at all times. This makes it harder to do something you didn't intend with a variable or chunk of data.

## .NET Core and .NET Standard

Throughout this book, you'll be learning about ASP.NET Core \(the web framework\). I'll occasionally mention the .NET runtime \(the supporting library that runs .NET code\).

You may also hear about .NET Core and .NET Standard. The naming gets confusing, so here's a simple explanation:

**.NET Standard** is a platform-agnostic interface that defines what features and APIs are available in .NET. .NET Standard doesn't represent any actual code or functionality, just the API definition. There are different "versions" or levels of .NET Standard that reflect how many APIs are available \(or how wide the API surface area is\). For example, .NET Standard 2.0 has more APIs available than .NET Standard 1.5, which has more APIs than .NET Standard 1.0.

**.NET Core** is the .NET runtime that can be installed on Windows, Mac, or Linux. It implements the APIs defined in the .NET Standard interface with the appropriate platform-specific code on each operating system. This is what you'll install on your own machine to build and run ASP.NET Core applications.

And just for good measure, **.NET Framework** is a different implementation of .NET Standard that is Windows-only. This was the only .NET runtime until .NET Core came along and opened .NET up to Mac and Linux. ASP.NET Core can also run on Windows-only .NET Framework, but I won't touch on this too much.

If you're confused by all this naming, no worries! We'll get to some real code in a bit.

## A note to ASP.NET 4 developers

If you haven't used a previous version of ASP.NET, skip ahead to the next chapter!

ASP.NET Core is a complete ground-up rewrite of ASP.NET, with a focus on modernizing the framework and finally decoupling it from System.Web, IIS, and Windows. If you remember all the OWIN/Katana stuff from ASP.NET 4, you're already halfway there: the Katana project became ASP.NET 5 which was ultimately renamed to ASP.NET Core.

Because of the Katana legacy, the `Startup` class is front and center, and there's no more `Application_Start` or `Global.asax`. The entire pipeline is driven by middleware, and there's no longer a split between MVC and Web API: controllers can simply return views, status codes, or data. Dependency injection comes baked in, so you don't need to install and configure a container like StructureMap or Ninject if you don't want to. And the entire framework has been optimized for speed and runtime efficiency.

Alright, enough introduction. Let's dive in to ASP.NET Core!

