# <a name="aspnet-core-authorization-sample"></a>Ejemplo de autorización de ASP.NET Core

Este ejemplo muestra el uso de la autorización de las páginas de Razor convenciones. Este ejemplo muestra las características descritas en la [convenciones de autorización de las páginas de Razor](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) tema.

Autorización del usuario en este ejemplo utiliza la autenticación de cookie características se describen en la [mediante la autenticación de Cookie sin ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) tema. Para obtener información sobre el uso de la identidad de núcleo de ASP.NET, vea los temas de la [autenticación](https://docs.microsoft.com/aspnet/core/security/authentication/index) sección de la documentación.

Al ejecutar el ejemplo, use la dirección de correo electrónico  **maria.rodriguez@contoso.com**  para autenticar al usuario.

## <a name="examples-in-this-sample"></a>En los ejemplos de este ejemplo

| Característica | Descripción |
| ------- | ----------- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Agrega un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página con la ruta de acceso especificada. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Agrega un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a todas las páginas en una carpeta con la ruta de acceso especificada. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Agrega un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una página con la ruta de acceso especificada. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Agrega un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a todas las páginas en una carpeta con la ruta de acceso especificada. |
