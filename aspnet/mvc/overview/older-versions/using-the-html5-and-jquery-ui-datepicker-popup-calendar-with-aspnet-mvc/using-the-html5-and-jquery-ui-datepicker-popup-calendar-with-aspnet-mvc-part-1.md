---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Usar el calendario HTML5 y jQuery UI Datepicker emergente con ASP.NET MVC - parte 1 | Documentos de Microsoft
author: Rick-Anderson
description: "Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de presentación y el calendario emergente de jQuery UI datepicker en una máquina virtual de ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 9320c8a2aadb3b3c5bd6cd90b59d8a72db384c0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Usar el calendario HTML5 y jQuery UI Datepicker emergente con ASP.NET MVC - parte 1
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de presentación y el calendario emergente de jQuery UI datepicker en una aplicación Web de MVC de ASP.NET.


Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de presentación y jQuery [calendario emergente de interfaz de usuario datepicker](http://plugins.jquery.com/project/datepicker) en una aplicación Web de MVC de ASP.NET. Para este tutorial, puede utilizar Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), que es una versión gratuita de Microsoft Visual Studio, o si ya tiene que puede usar Visual Studio 2010 SP1.

Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar el software necesario mediante los vínculos siguientes individualmente:

- [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)

Si usa Visual Studio 2010 en lugar de Visual Web Developer, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Este tutorial se da por supuesto que ha completado la [Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial o que está familiarizado con el desarrollo de ASP.NET MVC. Este tutorial comienza con el proyecto completado de la [Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial.

Este tutorial muestra el código en C#. Sin embargo, el [proyecto inicial](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) y proyecto completado también están disponibles en Visual Basic.

Un proyecto de Visual Studio con código fuente de C# y Visual Basic está disponible como acompañamiento de este tema: [descargar](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Lo que vamos a compilar

Podrá agregar plantillas (en concreto, editar y mostrar las plantillas) a la aplicación de película lista simple que se creó en el [Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial. También agregará un [datepicker de jQuery UI](http://jqueryui.com/demos/datepicker/) calendario emergente para simplificar el proceso de especificación de fechas. Captura de pantalla siguiente muestra la aplicación modificada con el calendario emergente de jQuery UI datepicker mostrado.

![termine de jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Obtendrá información de conocimientos

Esto es lo aprenderá:

- Cómo utilizar atributos de la [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) espacio de nombres para controlar el formato de datos cuando se muestre y cuando se encuentra en modo de edición.
- Cómo crear plantillas (Editar y mostrar las plantillas) para controlar el formato de datos.
- Cómo agregar la [datepicker de jQuery UI](http://jqueryui.com/demos/datepicker/) como una manera de especificar los campos de fecha.

### <a name="getting-started"></a>Introducción

Si aún no tiene la aplicación lista de película desde el proyecto de inicio, descárguelo mediante el siguiente vínculo: [descargar](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). A continuación, en el Explorador de Windows, haga clic en el *MvcMovie.zip* de archivos y seleccione **propiedades**. En el **MvcMovie.zip propiedades** cuadro de diálogo, seleccione **Unblock**. (Desbloqueo impide que una advertencia de seguridad que se produce cuando intenta utilizar un *.zip* archivo que ha descargado desde el web.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Haga clic en el *MvcMovie.zip* de archivos y seleccione **extraer todo** para descomprimir el archivo. En Visual Web Developer o Visual Studio 2010, abra el *MvcMovieCS\_TU.sln* archivo.

En **el Explorador de soluciones**, haga doble clic en el *Views\Shared\\_Layout.cshtml* para abrirlo. Cambiar el `H1` encabezado de **aplicación de MVC película** a **película jQuery**. Presione CTRL + F5 para ejecutar la aplicación y haga clic en el **inicio** ficha, que le llevará a la `Index` método del controlador de película. Para probar la aplicación, seleccione la **editar** vínculo y **detalles** vínculo por una de las películas. Tenga en cuenta que en las vistas de índice, editar y obtener más información, la fecha de lanzamiento y el precio son perfectamente con el formato:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

El formato de la fecha y el precio es el resultado del uso de la [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo en las propiedades de la `Movie` clase.

Abra la *Movie.cs* de archivos y marque como comentario el `DisplayFormat` del atributo en el `ReleaseDate` y `Price` propiedades. Resultante `Movie` clase tiene el siguiente aspecto:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Presione CTRL + F5 para ejecutar la aplicación y seleccione la **inicio** pestaña para ver la lista de películas. En este momento la fecha de lanzamiento muestra la fecha y hora, y el campo precio ya no muestra el símbolo de moneda. El cambio en la `Movie` clase deshacer el formato "nice" que vio anteriormente, pero se corregirá en un momento.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Con el atributo de tipo de datos de DataAnnotations para especificar el tipo de datos

Reemplace la comentada `DisplayFormat` de atributo para el `ReleaseDate` propiedad con el [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atributo, mediante el `Date` enumeración. Reemplace el `DisplayFormat` de atributo para el `Price` propiedad con el [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atributo nuevo, esta vez mediante el `Currency` enumeración. Este es el aspecto que tiene el código completo:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Ejecute la aplicación. La fecha de lanzamiento y las propiedades de precio se formatean ahora correctamente (es decir, con los formatos de fecha y moneda adecuados). El [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atributo proporciona metadatos de tipo de MVC de ASP.NET integrados plantillas para que los campos se representan en el formato correcto. Mediante el `DataType` atributo es preferible al uso de la `DisplayFormat` atributo que se encontraba originalmente en el código, porque el `DataType` atributo hace que el modelo más limpia y más flexibles para propósitos como internacionalización.

En la siguiente sección verá cómo crear plantillas personalizadas para mostrar los campos de fecha.

>[!div class="step-by-step"]
[Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
