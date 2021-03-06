---
title: "Convenciones de autorización de páginas de Razor en ASP.NET Core"
author: guardrex
description: "Obtener información sobre cómo controlar el acceso a las páginas con convenciones durante el inicio de autorizan a los usuarios y permitir que los usuarios anónimos pueden tener acceso a las páginas individuales o carpetas de páginas."
ms.author: riande
manager: wpickett
ms.date: 10/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 72b558816e687c30d0c60f2fd85227d0d803219b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Convenciones de autorización de páginas de Razor en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Una manera de controlar el acceso de la aplicación de las páginas de Razor es usar las convenciones de autorización en el inicio. Estas convenciones permiten autorizar a los usuarios y permitir que los usuarios anónimos pueden tener acceso a las páginas individuales o carpetas de páginas. Se aplican las convenciones descritas en este tema automáticamente [filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="require-authorization-to-access-a-page"></a>Requerir autorización para acceder a una página

Use la [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página en la ruta especificada:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz sin una extensión y que contiene solo barras diagonales.

Un [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Requerir autorización para acceder a una carpeta de páginas

Use la [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a todas las páginas en una carpeta en la ruta especificada:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.

Un [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.

## <a name="allow-anonymous-access-to-a-page"></a>Permitir el acceso anónimo a una página

Use la [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una página en la ruta especificada:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz sin una extensión y que contiene solo barras diagonales.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Permitir el acceso anónimo a una carpeta de páginas

Use la [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a todas las páginas en una carpeta en la ruta especificada:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Tenga en cuenta en combinar el acceso autorizado y anónimo

Es perfectamente válido para especificar que una carpeta de páginas requieren autorización y especificar que una página dentro de esa carpeta permite el acceso anónimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Sin embargo, lo contrario, no es cierto. No se puede declarar una carpeta de páginas para el acceso anónimo y especificar una página dentro de autorización:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Que requieren la autorización en la página privada no funcionará porque cuando tanto el `AllowAnonymousFilter` y `AuthorizeFilter` filtros se aplican a la página, el `AllowAnonymousFilter` wins y controla el acceso.

## <a name="see-also"></a>Vea también

* [Proveedores personalizados de rutas y modelos de página de páginas de Razor](xref:mvc/razor-pages/razor-pages-convention-features)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) (clase)
