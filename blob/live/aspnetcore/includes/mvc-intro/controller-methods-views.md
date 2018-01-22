
<span data-ttu-id="37885-101">En el próximo tutorial hablaremos de [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6).</span><span class="sxs-lookup"><span data-stu-id="37885-101">We'll cover [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) in the next tutorial.</span></span> <span data-ttu-id="37885-102">El atributo [Display](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) especifica qué se muestra como nombre de un campo (en este caso, "Release Date" en lugar de "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="37885-102">The [Display](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="37885-103">El atributo [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica el tipo de datos (Date), de modo que la información de hora almacenada en el campo no se muestra.</span><span class="sxs-lookup"><span data-stu-id="37885-103">The [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field is not displayed.</span></span>

<span data-ttu-id="37885-104">Vaya al controlador `Movies` y mantenga el puntero del mouse sobre un vínculo **Edit** (Editar) para ver la dirección URL de destino.</span><span class="sxs-lookup"><span data-stu-id="37885-104">Browse to the `Movies` controller and hold the mouse pointer over an **Edit** link to see the target URL.</span></span>

![Ventana del explorador con el mouse sobre el vínculo Edit (Editar) donde se muestra una dirección URL de vínculo http://localhost:1234/Movies/Edit/5](../../tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

<span data-ttu-id="37885-106">Los vínculos **Edit** (Editar), **Details** (Detalles) y **Delete** (Eliminar) se generan mediante la aplicación auxiliar de etiquetas de delimitador de MVC Core en el archivo *Views/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="37885-106">The **Edit**, **Details**, and **Delete** links are generated by the Core MVC Anchor Tag Helper in the *Views/Movies/Index.cshtml* file.</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

<span data-ttu-id="37885-107">Las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) permiten que el código de servidor participe en la creación y la representación de elementos HTML en archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="37885-107">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="37885-108">En el código anterior, `AnchorTagHelper` genera dinámicamente el valor del atributo HTML `href` a partir del identificador de ruta y el método de acción del controlador. Use **Ver código fuente** en su explorador preferido o use las herramientas de desarrollo para examinar el marcado generado.</span><span class="sxs-lookup"><span data-stu-id="37885-108">In the code above, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the controller action method and route id. You use **View Source** from your favorite browser or use the developer tools to examine the generated markup.</span></span> <span data-ttu-id="37885-109">A continuación se muestra una parte del HTML generado:</span><span class="sxs-lookup"><span data-stu-id="37885-109">A portion of the generated HTML is shown below:</span></span>

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

<span data-ttu-id="37885-110">Recupere el formato para el [enrutamiento](xref:mvc/controllers/routing) establecido en el archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="37885-110">Recall the format for [routing](xref:mvc/controllers/routing) set in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

<span data-ttu-id="37885-111">ASP.NET Core traduce `http://localhost:1234/Movies/Edit/4` en una solicitud al método de acción `Edit` del controlador `Movies` con el parámetro `Id` de 4.</span><span class="sxs-lookup"><span data-stu-id="37885-111">ASP.NET Core translates `http://localhost:1234/Movies/Edit/4` into a request to the `Edit` action method of the `Movies` controller with the parameter `Id` of 4.</span></span> <span data-ttu-id="37885-112">(Los métodos de controlador también se denominan métodos de acción).</span><span class="sxs-lookup"><span data-stu-id="37885-112">(Controller methods are also known as action methods.)</span></span>

<span data-ttu-id="37885-113">Las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) son una de las nuevas características más populares de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37885-113">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are one of the most popular new features in ASP.NET Core.</span></span> <span data-ttu-id="37885-114">Para más información, vea [Recursos adicionales](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="37885-114">See [Additional resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="37885-115">Abra el controlador `Movies` y examine los dos métodos de acción `Edit`.</span><span class="sxs-lookup"><span data-stu-id="37885-115">Open the `Movies` controller and examine the two `Edit` action methods.</span></span> <span data-ttu-id="37885-116">En el código siguiente se muestra el método `HTTP GET Edit`, que captura la película y rellena el formulario de edición generado por el archivo de Razor *Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="37885-116">The following code shows the `HTTP GET Edit` method, which fetches the movie and populates the edit form generated by the *Edit.cshtml* Razor file.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

<span data-ttu-id="37885-117">En el código siguiente se muestra el método `HTTP POST Edit`, que procesa los valores de película publicados:</span><span class="sxs-lookup"><span data-stu-id="37885-117">The following code shows the `HTTP POST Edit` method, which processes the posted movie values:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

<span data-ttu-id="37885-118">El atributo `[Bind]` es una manera de proteger contra el [exceso de publicación](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost).</span><span class="sxs-lookup"><span data-stu-id="37885-118">The `[Bind]` attribute is one way to protect against [over-posting](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost).</span></span> <span data-ttu-id="37885-119">Solo debe incluir propiedades en el atributo `[Bind]` que quiera cambiar.</span><span class="sxs-lookup"><span data-stu-id="37885-119">You should only include properties in the `[Bind]` attribute that you want to change.</span></span> <span data-ttu-id="37885-120">Para más información, vea [Protección del controlador frente al exceso de publicación](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="37885-120">See [Protect your controller from over-posting](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application) for more information.</span></span> <span data-ttu-id="37885-121">[ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) ofrece un enfoque alternativo para evitar el exceso de publicaciones.</span><span class="sxs-lookup"><span data-stu-id="37885-121">[ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) provide an alternative approach to prevent over-posting.</span></span>

<span data-ttu-id="37885-122">Observe que el segundo método de acción `Edit` va precedido del atributo `[HttpPost]`.</span><span class="sxs-lookup"><span data-stu-id="37885-122">Notice the second `Edit` action method is preceded by the `[HttpPost]` attribute.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

<span data-ttu-id="37885-123">El atributo `HttpPost` especifica que este método `Edit` se puede invocar *solamente* para solicitudes `POST`.</span><span class="sxs-lookup"><span data-stu-id="37885-123">The `HttpPost` attribute specifies that this `Edit` method can be invoked *only* for `POST` requests.</span></span> <span data-ttu-id="37885-124">Podría aplicar el atributo `[HttpGet]` al primer método de edición, pero no es necesario hacerlo porque `[HttpGet]` es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="37885-124">You could apply the `[HttpGet]` attribute to the first edit method, but that's not necessary because `[HttpGet]` is the default.</span></span>

<span data-ttu-id="37885-125">El atributo `ValidateAntiForgeryToken` se usa para [impedir la falsificación de una solicitud](xref:security/anti-request-forgery) y se empareja con un token antifalsificación generado en el archivo de vista de edición (*Views/Movies/Edit.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="37885-125">The `ValidateAntiForgeryToken` attribute is used to [prevent forgery of a request](xref:security/anti-request-forgery) and is paired up with an anti-forgery token generated in the edit view file (*Views/Movies/Edit.cshtml*).</span></span> <span data-ttu-id="37885-126">El archivo de vista de edición genera el token antifalsificación con la [aplicación auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms).</span><span class="sxs-lookup"><span data-stu-id="37885-126">The edit view file generates the anti-forgery token with the [Form Tag Helper](xref:mvc/views/working-with-forms).</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

<span data-ttu-id="37885-127">La [aplicación auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms) genera un token antifalsificación oculto que debe coincidir con el token antifalsificación generado por `[ValidateAntiForgeryToken]` en el método `Edit` del controlador Movies.</span><span class="sxs-lookup"><span data-stu-id="37885-127">The [Form Tag Helper](xref:mvc/views/working-with-forms) generates a hidden anti-forgery token that must match the `[ValidateAntiForgeryToken]` generated anti-forgery token in the `Edit` method of the Movies controller.</span></span> <span data-ttu-id="37885-128">Para más información, vea [Prevención de ataques de falsificación de solicitudes](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="37885-128">For more information, see [Anti-Request Forgery](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="37885-129">El método `HttpGet Edit` toma el parámetro `ID` de la película, busca la película con el método `SingleOrDefaultAsync` de Entity Framework y devuelve la película seleccionada a la vista de edición.</span><span class="sxs-lookup"><span data-stu-id="37885-129">The `HttpGet Edit` method takes the movie `ID` parameter, looks up the movie using the Entity Framework `SingleOrDefaultAsync` method, and returns the selected movie to the Edit view.</span></span> <span data-ttu-id="37885-130">Si no se encuentra una película, se devuelve `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="37885-130">If a movie cannot be found, `NotFound` (HTTP 404) is returned.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

<span data-ttu-id="37885-131">Cuando el sistema de scaffolding creó la vista de edición, examinó la clase `Movie` y creó código para representar los elementos `<label>` y `<input>` para cada propiedad de la clase.</span><span class="sxs-lookup"><span data-stu-id="37885-131">When the scaffolding system created the Edit view, it examined the `Movie` class and created code to render `<label>` and `<input>` elements for each property of the class.</span></span> <span data-ttu-id="37885-132">En el ejemplo siguiente se muestra la vista de edición que generó el sistema de scaffolding de Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="37885-132">The following example shows the Edit view that was generated by the Visual Studio scaffolding system:</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/EditCopy.cshtml?highlight=1)]

<span data-ttu-id="37885-133">Observe cómo la plantilla de vista tiene una instrucción `@model MvcMovie.Models.Movie` en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="37885-133">Notice how the view template has a `@model MvcMovie.Models.Movie` statement at the top of the file.</span></span> <span data-ttu-id="37885-134">`@model MvcMovie.Models.Movie` especifica que la vista espera que el modelo de la plantilla de vista sea del tipo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="37885-134">`@model MvcMovie.Models.Movie` specifies that the view expects the model for the view template to be of type `Movie`.</span></span>

<span data-ttu-id="37885-135">El código con scaffolding usa varios métodos de aplicación auxiliar de etiquetas para simplificar el marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="37885-135">The scaffolded code uses several Tag Helper methods to streamline the HTML markup.</span></span> <span data-ttu-id="37885-136">La [aplicación auxiliar de etiquetas](xref:mvc/views/working-with-forms) muestra el nombre del campo: "Title" (Título), "ReleaseDate" (Fecha de lanzamiento), "Genre" (Género) o "Price" (Precio).</span><span class="sxs-lookup"><span data-stu-id="37885-136">The - [Label Tag Helper](xref:mvc/views/working-with-forms) displays the name of the field ("Title", "ReleaseDate", "Genre", or "Price").</span></span> <span data-ttu-id="37885-137">La [aplicación auxiliar de etiquetas de entrada](xref:mvc/views/working-with-forms) representa un elemento HTML `<input>`.</span><span class="sxs-lookup"><span data-stu-id="37885-137">The [Input Tag Helper](xref:mvc/views/working-with-forms) renders an HTML `<input>` element.</span></span> <span data-ttu-id="37885-138">La [aplicación auxiliar de etiquetas de validación](xref:mvc/views/working-with-forms) muestra cualquier mensaje de validación asociado a esa propiedad.</span><span class="sxs-lookup"><span data-stu-id="37885-138">The [Validation Tag Helper](xref:mvc/views/working-with-forms) displays any validation messages associated with that property.</span></span>

<span data-ttu-id="37885-139">Ejecute la aplicación y navegue a la URL `/Movies`.</span><span class="sxs-lookup"><span data-stu-id="37885-139">Run the application and navigate to the `/Movies` URL.</span></span> <span data-ttu-id="37885-140">Haga clic en un vínculo **Edit** (Editar).</span><span class="sxs-lookup"><span data-stu-id="37885-140">Click an **Edit** link.</span></span> <span data-ttu-id="37885-141">En el explorador, vea el código fuente de la página.</span><span class="sxs-lookup"><span data-stu-id="37885-141">In the browser, view the source for the page.</span></span> <span data-ttu-id="37885-142">El código HTML generado para el elemento `<form>` se muestra abajo.</span><span class="sxs-lookup"><span data-stu-id="37885-142">The generated HTML for the `<form>` element is shown below.</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

<span data-ttu-id="37885-143">Los elementos `<input>` se muestran en un elemento `HTML <form>` cuyo atributo `action` se establece para publicar en la dirección URL `/Movies/Edit/id`.</span><span class="sxs-lookup"><span data-stu-id="37885-143">The `<input>` elements are in an `HTML <form>` element whose `action` attribute is set to post to the `/Movies/Edit/id` URL.</span></span> <span data-ttu-id="37885-144">Los datos del formulario se publicarán en el servidor cuando se haga clic en el botón `Save`.</span><span class="sxs-lookup"><span data-stu-id="37885-144">The form data will be posted to the server when the `Save` button is clicked.</span></span> <span data-ttu-id="37885-145">La última línea antes del cierre del elemento `</form>` muestra el token [XSRF](xref:security/anti-request-forgery) oculto generado por la [aplicación auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms).</span><span class="sxs-lookup"><span data-stu-id="37885-145">The last line before the closing `</form>` element shows the hidden [XSRF](xref:security/anti-request-forgery) token generated by the [Form Tag Helper](xref:mvc/views/working-with-forms).</span></span>

## <a name="processing-the-post-request"></a><span data-ttu-id="37885-146">Procesamiento de la solicitud POST</span><span class="sxs-lookup"><span data-stu-id="37885-146">Processing the POST Request</span></span>

<span data-ttu-id="37885-147">En la siguiente lista se muestra la versión `[HttpPost]` del método de acción `Edit`.</span><span class="sxs-lookup"><span data-stu-id="37885-147">The following listing shows the `[HttpPost]` version of the `Edit` action method.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

<span data-ttu-id="37885-148">El atributo `[ValidateAntiForgeryToken]` valida el token [XSRF](xref:security/anti-request-forgery) oculto generado por el generador de tokens antifalsificación en la [herramienta auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms).</span><span class="sxs-lookup"><span data-stu-id="37885-148">The `[ValidateAntiForgeryToken]` attribute validates the hidden [XSRF](xref:security/anti-request-forgery) token generated by the anti-forgery token generator in the [Form Tag Helper](xref:mvc/views/working-with-forms)</span></span>

<span data-ttu-id="37885-149">El sistema de [enlace de modelos](xref:mvc/models/model-binding) toma los valores de formulario publicados y crea un objeto `Movie` que se pasa como el parámetro `movie`.</span><span class="sxs-lookup"><span data-stu-id="37885-149">The [model binding](xref:mvc/models/model-binding) system takes the posted form values and creates a `Movie` object that's passed as the `movie` parameter.</span></span> <span data-ttu-id="37885-150">El método `ModelState.IsValid` comprueba que los datos presentados en el formulario pueden usarse para modificar (editar o actualizar) un objeto `Movie`.</span><span class="sxs-lookup"><span data-stu-id="37885-150">The `ModelState.IsValid` method verifies that the data submitted in the form can be used to modify (edit or update) a `Movie` object.</span></span> <span data-ttu-id="37885-151">Si los datos son válidos, se guardan.</span><span class="sxs-lookup"><span data-stu-id="37885-151">If the data is valid it's saved.</span></span> <span data-ttu-id="37885-152">Los datos de película actualizados (o modificados) se guardan en la base de datos mediante una llamada al método `SaveChangesAsync` del contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="37885-152">The updated (edited) movie data is saved to the database by calling the `SaveChangesAsync` method of database context.</span></span> <span data-ttu-id="37885-153">Después de guardar los datos, el código redirige al usuario al método de acción `Index` de la clase `MoviesController`, que muestra la colección de películas, incluidos los cambios que se acaban de hacer.</span><span class="sxs-lookup"><span data-stu-id="37885-153">After saving the data, the code redirects the user to the `Index` action method of the `MoviesController` class, which displays the movie collection, including the changes just made.</span></span>

<span data-ttu-id="37885-154">Antes de que el formulario se envíe al servidor, la validación del lado cliente comprueba cualquier regla de validación en los campos.</span><span class="sxs-lookup"><span data-stu-id="37885-154">Before the form is posted to the server, client side validation checks any validation rules on the fields.</span></span> <span data-ttu-id="37885-155">Si hay errores de validación, se muestra un mensaje de error y no se publica el formulario.</span><span class="sxs-lookup"><span data-stu-id="37885-155">If there are any validation errors, an error message is displayed and the form is not posted.</span></span> <span data-ttu-id="37885-156">Si JavaScript está deshabilitado, no dispondrá de la validación del lado cliente, sino que el servidor detectará los valores publicados que no son válidos y los valores de formulario se volverán a mostrar con mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="37885-156">If JavaScript is disabled, you won't have client side validation but the server will detect the posted values that are not valid, and the form values will be redisplayed with error messages.</span></span> <span data-ttu-id="37885-157">Más adelante en el tutorial se examina la [validación de modelos](xref:mvc/models/validation) con más detalle.</span><span class="sxs-lookup"><span data-stu-id="37885-157">Later in the tutorial we examine [Model Validation](xref:mvc/models/validation) in more detail.</span></span> <span data-ttu-id="37885-158">La [aplicación auxiliar de etiquetas de validación](xref:mvc/views/working-with-forms) en la plantilla de vista *Views/Movies/Edit.cshtml* se encarga de mostrar los mensajes de error correspondientes.</span><span class="sxs-lookup"><span data-stu-id="37885-158">The [Validation Tag Helper](xref:mvc/views/working-with-forms) in the *Views/Movies/Edit.cshtml* view template takes care of displaying appropriate error messages.</span></span>

![Vista de edición: excepción de un valor de precio incorrecto de los estados abc que indica que el campo de precio debe ser un número.](../../tutorials/first-mvc-app/controller-methods-views/_static/val.png)

<span data-ttu-id="37885-161">Todos los métodos `HttpGet` del controlador de películas siguen un patrón similar.</span><span class="sxs-lookup"><span data-stu-id="37885-161">All the `HttpGet` methods in the movie controller follow a similar pattern.</span></span> <span data-ttu-id="37885-162">Obtienen un objeto de película (o una lista de objetos, en el caso de `Index`) y pasan el objeto (modelo) a la vista.</span><span class="sxs-lookup"><span data-stu-id="37885-162">They get a movie object (or list of objects, in the case of `Index`), and pass the object (model) to the view.</span></span> <span data-ttu-id="37885-163">El método `Create` pasa un objeto de película vacío a la vista `Create`.</span><span class="sxs-lookup"><span data-stu-id="37885-163">The `Create` method passes an empty movie object to the `Create` view.</span></span> <span data-ttu-id="37885-164">Todos los métodos que crean, editan, eliminan o modifican los datos lo hacen en la sobrecarga `[HttpPost]` del método.</span><span class="sxs-lookup"><span data-stu-id="37885-164">All the methods that create, edit, delete, or otherwise modify data do so in the `[HttpPost]` overload of the method.</span></span> <span data-ttu-id="37885-165">La modificación de datos en un método `HTTP GET` supone un riesgo de seguridad.</span><span class="sxs-lookup"><span data-stu-id="37885-165">Modifying data in an `HTTP GET` method is a security risk.</span></span> <span data-ttu-id="37885-166">La modificación de datos en un método `HTTP GET` también infringe procedimientos recomendados de HTTP y el patrón de arquitectura [REST](http://rest.elkstein.org/), que especifica que las solicitudes GET no deben cambiar el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37885-166">Modifying data in an `HTTP GET` method also violates HTTP best practices and the architectural [REST](http://rest.elkstein.org/) pattern, which specifies that GET requests should not change the state of your application.</span></span> <span data-ttu-id="37885-167">En otras palabras, realizar una operación GET debería ser una operación segura sin efectos secundarios, que no modifica los datos persistentes.</span><span class="sxs-lookup"><span data-stu-id="37885-167">In other words, performing a GET operation should be a safe operation that has no side effects and doesn't modify your persisted data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37885-168">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="37885-168">Additional resources</span></span>

* [<span data-ttu-id="37885-169">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="37885-169">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="37885-170">Introducción a las aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="37885-170">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="37885-171">Creación de aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="37885-171">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)
* [<span data-ttu-id="37885-172">Prevención de ataques de falsificación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="37885-172">Anti-Request Forgery</span></span>](xref:security/anti-request-forgery)
* <span data-ttu-id="37885-173">Protección del controlador frente al [exceso de publicación](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)</span><span class="sxs-lookup"><span data-stu-id="37885-173">Protect your controller from [over-posting](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)</span></span>
* [<span data-ttu-id="37885-174">ViewModels</span><span class="sxs-lookup"><span data-stu-id="37885-174">ViewModels</span></span>](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [<span data-ttu-id="37885-175">Aplicación auxiliar de etiquetas de formulario</span><span class="sxs-lookup"><span data-stu-id="37885-175">Form Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="37885-176">Aplicación auxiliar de etiquetas de entrada</span><span class="sxs-lookup"><span data-stu-id="37885-176">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="37885-177">Aplicación auxiliar de etiquetas de elementos de etiqueta</span><span class="sxs-lookup"><span data-stu-id="37885-177">Label Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="37885-178">Aplicación auxiliar de etiquetas de selección</span><span class="sxs-lookup"><span data-stu-id="37885-178">Select Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="37885-179">Aplicación auxiliar de etiquetas de validación</span><span class="sxs-lookup"><span data-stu-id="37885-179">Validation Tag Helper</span></span>](xref:mvc/views/working-with-forms)