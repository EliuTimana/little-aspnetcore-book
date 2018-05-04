# Crear una Vista

Las vistas en ASP.NET Core están construidas con un lenguaje de plantillas denominado Razor, el cual combina el uso de código HTML y C\#. \(Si ya has trabajado con ERB en Ruby On Rails, Handlebars con Mustache o Thymeleaf en Java, ya tendrás una idea acerca de este tipo de lenguajes.\)

La mayor parte del código es HTML, con algún fragmento de código C\# utilizado para obtener información del modelo y añadirla de forma dinámica a la vista del modelo como texto ó HTML. Los fragmentos de código C\# llevan el prefijo `@`.

La vista que es renderizada por la acción `Index` del controlador `TodoController` necesita insertar la información obtenida en la vista del modelo \(en este caso una secuencia de items de la aplicación to-do\) y mostrarla para el usuario dentro de una tabla. Por convención, las vistas están localizadas en el directorio `Views` , en un subdirectorio con el nombre del correspondiente controlador. El nombre del fichero de la vista es el nombre de la acción con la extensión `.cshtml`.

Crear un directorio llamado `Todo` dentro del directorio `Views`, y añade este código:

`Views/Todo/Index.cshtml`

```markup
@model TodoViewModel

@{
    ViewData["Title"] = "Manage your todo list";
}

<div class="panel panel-default todo-panel">
  <div class="panel-heading">@ViewData["Title"]</div>

  <table class="table table-hover">
      <thead>
          <tr>
              <td>&#x2714;</td>
              <td>Item</td>
              <td>Due</td>
          </tr>
      </thead>

      @foreach (var item in Model.Items)
      {
          <tr>
              <td>
                <input type="checkbox" class="done-checkbox">
              </td>
              <td>@item.Title</td>
              <td>@item.DueAt</td>
          </tr>
      }
  </table>

  <div class="panel-footer add-item-form">
    <!-- TODO: Añadir el formulario para añadir items -->
  </div>
</div>
```

En la parte superior del fichero, la directiva `@model` le proporciona información a Razor indicando que modelo debe esperar y  utilizar esta vista. El modelo es accesible a través de la propiedad `Model`.

Asumiendo que existirán items en  `Model.Items`, la declaración `foreach` nos permitirá iterar sobre cada unos de ellos y renderizar la fila de la tabla el \(elemento `<tr>`\) conteniendo el nombre y fecha de creación de cada item. Ademas es también renderizado un checkbox que nos permitirá marcar como completado un item si lo creemos necesario.

## El fichero layout

Quizá te estes preguntando donde esta el resto del fichero HTML? Donde se encuentra la etiqueta `<body>`? El header o el footer de la pagina? ASP.NET Core utiliza una vista denominada layout que nos permite definir una estructura básica para cada pagina HTML dentro de nuestra aplicación. Este layout esta ubicado en la siguiente ruta `Views/Shared/_Layout.cshtml`.

Por defecto la plantilla ASP.NET Core incluye Bootstrap y jQuery dentro de este layout, de tal forma que de una forma rapida y sencilla puedas crear un aplicación web. Por supuesto siempre tendrás la posibilidad de utilizar tus propias hojas de estilos y librerías JavaScript si lo así lo cress necesario.

## Personalizar las hoja de estilos

La plantillas por defecto ademas incluye una hoja de estilos con algunas reglas CSS básicas. Esta hoja de estilos esta ubicada en el directorio `wwwroot/css`. Por favor añade la siguientes reglas CSS al final del fichero`site.css`:

`wwwroot/css/site.css`

```css
div.todo-panel {
  margin-top: 15px;
}

table tr.done {
  text-decoration: line-through;
  color: #888;
}
```

Podrás utilizar reglas CSS como estas para personalizar completamente el aspecto de tus paginas web.

Como veremos mas adelante ASP.NET Core y Razor nos permiten hacer esto y mucho más, como por ejemplo hacer uso de vistas parciales y vistas basadas en componentes, pero por ahora con este sencillo layout y una vista es todo lo que necesitamos. La documentación oficial de ASP.NET Core \([https://docs.asp.net](https://docs.asp.net)\) contiene un gran variedad de ejemplos si tienes interés en profundizar un poco mas sobre este tema.

