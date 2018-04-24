# Crear los Modelos

Hay 2 tipos diferentes de clases de modelos que necesitan ser creadas: un modelo que representa un item de tipo to-do en la base de datos \(usualmente denominado **entidad**\) y un modelo que se combinara con una vista \(las siglas **MV** en MVC\) y sera enviado de vuelta al navegador web del usuario. Dado que ambos modelos pueden ser denominados como "modelos", yo prefiero denominador a este ultimo como **modelo de vista**.

En primer lugar, crea una clase denominada`TodoItem` en el directorio de los modelos:

`Models/TodoItem.cs`

```csharp
using System;

namespace AspNetCoreTodo.Models
{
    public class TodoItem
    {
        public Guid Id { get; set; }

        public bool IsDone { get; set; }

        public string Title { get; set; }

        public DateTimeOffset? DueAt { get; set; }
    }
}
```

Esta clase define que sera necesario almacenar en la base de datos para cada item de tipo to-do, es decir: un ID, un titulo o nombre, si el item esta completado y por ultimo cual es su fecha de creación . Cada linea  del código superior define una propiedad de la clase:

* La propiedad **Id** es de tipo guid \(**g**lobally **u**nique **id**entifier\). Guids \(o GUIDs\) son strings de gran tamaño formados por letras y números, como por ejemplo `43ec09f2-7f70-4f4b-9559-65011d5781bb`. Debido a que estos guids son aleatorios y que rara vez se duplican, son muy utilizados para generar identificadores únicos. Podrías utilizar un numero entero como identificador de la entidad dentro de la base de datos, pero entonces necesitarías configurar también el auto incremento del identificador cada vez que añadieras un elemento en la base de datos. Los GUIDS son generados de manera aleatoria , por lo que no tienes que preocuparte de este auto incremento.
* La propiedad **IsDone** es de tipo boleano \(valor verdadero/false\). Por defecto siempre sera `falso` cuando se creen nuevos items. Mas adelante harás uso de código para cambiar el estado de la propiedad a `verdadero` cuando el usuario haga clic en el checkbox del item dentro de la vista .
* La propiedad **Title** es e tipo string. En ella se almacenara el nombre o breve descripción de cada item de tipo to-do.
* La propiedad **DueAt** es de tipo`DateTimeOffset`, el cual almacena una fecha junto con su zona horaria respecto al UTC. Almacenar juntos la fecha, la hora y la zona horaria permite renderizar de una forma mas sencilla diferentes fechas en diferentes zonas horarias.

Te das cuenta del`?` `DateTimeOffset` ? Esa marca nos indica que es propiedad puede ser **nula**, o lo que es lo mismo opcional. Si el `?` no estuviera incluido, cada item debería tener especificado una fecha de creación. Las propiedades `Id` e`IsDone` no están definidas como opcionales,  por lo que son obligatorias y por tanto siempre deben tener un valor \(o valor por defecto\).

> Los Strings en C\# siempre son de tipo opcional, por lo que no es necesario marcar la propiedad Title con ?. Los strings en C\# pueden ser null, vacíos o contener texto.

Cada una de propiedades arriba definidas esta seguida del codigo`get; set;`, el cual es un atajo para indicar que son de tipo lectura/escritura \(o siendo más técnicos, que tiene definidos unos métodos denominados getters y setters\).

Llegados a este punto, ya no es relevante el tipo de tecnología que se este utilizando en las bases de datos. Esta podría ser de tipo SQL Server, MySQL, MongoDB, Redis, o incluso algo mas exótico. El modelo se encarga de definir en codigo C\# el formato de la entidad de tal forma que no tengas que preocuparte como funciona la base de datos a bajo nivel. Este estilo simple de crear lo modelos suele ser denominado "Plain Old C\# Object" o **POCO**.

## El modelo de la vista

Frecuentemente, el modelo \(entidad\) que almacenas en la base de datos es muy similar pero no exactamente el mismo al que necesitas utilizar en MVC \(el modelo de la vista\). En este caso, el modelo `TodoItem` representa a un único item en la base de datos, pero la vista posiblemente necesite 2, 10, o cientos items.

Como consecuencia de esto, el modelo de la vista debe ser una clase independiente que almacene un array de`TodoItem`s:

`Models/TodoViewModel.cs`

```csharp
namespace AspNetCoreTodo.Models
{
    public class TodoViewModel
    {
        public TodoItem[] Items { get; set; }
    }
}
```

Ahora que ya dispones de los modelos, es tiempo de crear una vista que haga usado del modelo`TodoViewModel` y genere el HTML correcto para mostrar al usuario la lista de To-Dos.

