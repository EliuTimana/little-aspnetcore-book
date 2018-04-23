# Conceptos básicos sobre MVC

En este capitulo, exploraras la arquitectura MVC en ASP.NET Core. **MVC** \(Modelo-Vista-Controlador\) es un patrón para la creación de aplicaciones web que es utilizado prácticamente en la totalidad de los frameworks web actuales \(Ruby on Rails y Express son posiblemente los ejemplos de back-end mas populares\), ademas frameworks front-end como Angular. Aplicaciones móviles en iOS y Android también utilizan una variante de MVC .

Como su propio nombre indica, MVC esta compuesto por 3 componentes: modelos, vistas y controladores. Los **Controladores** se encargan de manejar todas las peticiones entrantes desde un cliente o un navegador web y tomar decisiones acerca de que código ejecutar en cada momento. Las **Vistas** son plantillas \(normalmente en HTML más un lenguaje de plantillas como puede ser Handlebars, Pug, o Razor\) a las cuales se les añade información y posteriormente se muestran al usuario. Los **Modelos** contienen la información que es añadida a las  vistas, o la información que es enviada por el cliente.

Un patrón común en MVC seria:

* El controlador recibe una petición y busca cierta información en una base de datos.
* El controlador crea un modelo con la información obtenida y lo añade a una vista.
* La vista es renderizada y mostrada en el navegador del usuario.
* El usuario hace clic en un botón o envía un formulario, lo cual genera otra petición al controlador y el ciclo se vuelve a repetir.

Si has trabajado con MVC en otros lenguajes, te sentirás como en casa con el patrón  MVC de ASP.NET Core. Si eres nuevo en MVC, este capitulo te ensañara los conceptos básicos y te ayudara a iniciarte.

## Que vas a construir

El ejercicio típico "Hello World" para MVC  es construir una aplicación de tipo lista To-Do. Es un muy buen proyecto para comenzar ya que pese a ser pequeño y simple en su alcance, toca todos los componentes del modelo MVC y ademas cubre todo los conceptos que necesitarías utilizar en un aplicación a grande escala.

En este libro, construirás una aplicación To-Do que permitirá al usuario añadir items a la lista de to-dos y marcarlos cuando estén completos. Más detalladamente, vas a crear:

* Una aplicación web de tipo servidor \(normalmente denominado "back-end"\) utilizando ASP.NET Core, C\# y el patrón MVC.
* Una base de datos para almacenar los items del usuario utilizando el motor de bases SQLite y un sistema denominado Entity Framework Core.
* Paginas web y una interfaz con la que el usuario interectuará a través de su navegador web, haciendo uso de HTML, CSS, y JavaScript \(esta parte se suele denominar "front-end"\).
* Un formulario para iniciar sesión y controles de seguridad de tal forma que los items de cada usuario sean privados para el resto.

Suena  bien verdad? Pongámonos manos a la obra! Si aun no has creado el proyecto ASP.NET Core usando `dotnet new mvc`, sigue los pasos indicados en el capitulo anterior. Deberías ser capaz de crear y ejecutar el proyecto y ver la pagina creada por  defecto.

