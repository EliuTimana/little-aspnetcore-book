# Instala el SDK

Simplemente busca en Google "**download .net core**"  y sigue la instrucciones de instalación en la pagina de descargas de Microsoft para obtener el SDK de .NET Core. Una vez que la instalación haya finalizado, abre tu terminal favorito \(o en el caso de windows el  PowerShell \) y haz uso del comando`dotnet` para asegurarte de que todo funciona correctamente:

```text
dotnet --version

2.1.104
```

Con el flag `--info` puedes obtener mas información acerca de la plataforma:

```text
dotnet --info

.NET Command Line Tools (2.1.104)

Product Information:
 Version:            2.1.104
 Commit SHA-1 hash:  48ec687460

Runtime Environment:
 OS Name:     Mac OS X
 OS Version:  10.13

(more details...)
```

Si el resultado del comando coincide con el bloque de código superior, estas listo para continuar !

