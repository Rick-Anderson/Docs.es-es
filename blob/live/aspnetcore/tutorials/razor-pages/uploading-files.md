---
title: "Carga de archivos en una página de Razor en ASP.NET Core"
author: guardrex
description: "Aprenda a cargar archivos en una página de Razor."
keywords: "ASP.NET Core,Razor,páginas de Razor,IFormFile,carga de archivos,fileupload"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 3b54bf0b40c396c8c141966219f65231fb362ca4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="bae3b-104">Carga de archivos en una página de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bae3b-104">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="bae3b-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bae3b-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bae3b-106">En esta sección se muestra cómo cargar archivos con una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="bae3b-106">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="bae3b-107">La [aplicación de ejemplo Razor Pages Movie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) de este tutorial usa el enlace de modelo simple para cargar archivos, lo que funciona bien para cargar archivos pequeños.</span><span class="sxs-lookup"><span data-stu-id="bae3b-107">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="bae3b-108">Para más información sobre la transmisión de archivos de gran tamaño, vea [Uploading large files with streaming (Carga de archivos grandes mediante transmisión)](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="bae3b-108">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="bae3b-109">En los pasos siguientes se agrega una característica de carga de archivo de programación de película a la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="bae3b-109">In the steps below, you add a movie schedule file upload feature to the sample app.</span></span> <span data-ttu-id="bae3b-110">Una programación de película está representada por una clase `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="bae3b-110">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="bae3b-111">La clase incluye dos versiones de la programación.</span><span class="sxs-lookup"><span data-stu-id="bae3b-111">The class includes two versions of the schedule.</span></span> <span data-ttu-id="bae3b-112">Una versión se proporciona a los clientes, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="bae3b-112">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="bae3b-113">La otra se usa para los empleados de la empresa, `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="bae3b-113">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="bae3b-114">Cada versión se carga como un archivo independiente.</span><span class="sxs-lookup"><span data-stu-id="bae3b-114">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="bae3b-115">El tutorial muestra cómo realizar dos cargas de archivos desde una página con un solo elemento POST en el servidor.</span><span class="sxs-lookup"><span data-stu-id="bae3b-115">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="bae3b-116">Adición de una clase FileUpload</span><span class="sxs-lookup"><span data-stu-id="bae3b-116">Add a FileUpload class</span></span>

<span data-ttu-id="bae3b-117">A continuación, cree una página de Razor para controlar un par de cargas de archivos.</span><span class="sxs-lookup"><span data-stu-id="bae3b-117">Below, you create a Razor page to handle a pair of file uploads.</span></span> <span data-ttu-id="bae3b-118">Agregue una clase `FileUpload`, que está enlazada a la página para obtener los datos de programación.</span><span class="sxs-lookup"><span data-stu-id="bae3b-118">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="bae3b-119">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="bae3b-119">Right click the *Models* folder.</span></span> <span data-ttu-id="bae3b-120">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="bae3b-120">Select **Add** > **Class**.</span></span> <span data-ttu-id="bae3b-121">Asigne a la clase el nombre **FileUpload** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="bae3b-121">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="bae3b-122">La clase tiene una propiedad para el título de la programación y otra para cada una de las dos versiones de la programación.</span><span class="sxs-lookup"><span data-stu-id="bae3b-122">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="bae3b-123">Las tres propiedades son necesarias y el título debe tener entre 3 y 60 caracteres.</span><span class="sxs-lookup"><span data-stu-id="bae3b-123">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="bae3b-124">Agregar un método auxiliar para cargar archivos</span><span class="sxs-lookup"><span data-stu-id="bae3b-124">Add a helper method to upload files</span></span>

<span data-ttu-id="bae3b-125">Para evitar la duplicación de código para el procesamiento de archivos de programación cargados, primero agregue un método auxiliar estático.</span><span class="sxs-lookup"><span data-stu-id="bae3b-125">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="bae3b-126">Cree una carpeta *Utilities* en la aplicación y agregue un archivo *FileHelpers.cs* con el siguiente contenido.</span><span class="sxs-lookup"><span data-stu-id="bae3b-126">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="bae3b-127">El método auxiliar, `ProcessFormFile`, toma un elemento [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) y [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) y devuelve una cadena con el contenido y el tamaño del archivo.</span><span class="sxs-lookup"><span data-stu-id="bae3b-127">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="bae3b-128">Se comprueban el tipo de contenido y la longitud.</span><span class="sxs-lookup"><span data-stu-id="bae3b-128">The content type and length are checked.</span></span> <span data-ttu-id="bae3b-129">Si el archivo no pasa una comprobación de validación, se agrega un error a `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="bae3b-129">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a><span data-ttu-id="bae3b-130">Adición de la clase Schedule</span><span class="sxs-lookup"><span data-stu-id="bae3b-130">Add the Schedule class</span></span>

<span data-ttu-id="bae3b-131">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="bae3b-131">Right click the *Models* folder.</span></span> <span data-ttu-id="bae3b-132">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="bae3b-132">Select **Add** > **Class**.</span></span> <span data-ttu-id="bae3b-133">Asigne a la clase el nombre **Schedule** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="bae3b-133">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="bae3b-134">La clase usa los atributos `Display` y `DisplayFormat`, que generan títulos descriptivos y formato cuando se representan los datos de programación.</span><span class="sxs-lookup"><span data-stu-id="bae3b-134">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="bae3b-135">Actualización de MovieContext</span><span class="sxs-lookup"><span data-stu-id="bae3b-135">Update the MovieContext</span></span>

<span data-ttu-id="bae3b-136">Especifique `DbSet` en `MovieContext` (*Models/MovieContext.cs*) para las programaciones:</span><span class="sxs-lookup"><span data-stu-id="bae3b-136">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="bae3b-137">Adición de la tabla Schedule a la base de datos</span><span class="sxs-lookup"><span data-stu-id="bae3b-137">Add the Schedule table to the database</span></span>

<span data-ttu-id="bae3b-138">Abra la Consola del Administrador de paquetes (PMC): **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="bae3b-138">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="bae3b-140">En la PMC, ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="bae3b-140">In the PMC, execute the following commands.</span></span> <span data-ttu-id="bae3b-141">Estos comandos agregan una tabla `Schedule` a la base de datos:</span><span class="sxs-lookup"><span data-stu-id="bae3b-141">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="bae3b-142">Adición de una página de Razor de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="bae3b-142">Add a file upload Razor Page</span></span>

<span data-ttu-id="bae3b-143">En la carpeta *Pages*, cree una carpeta *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="bae3b-143">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="bae3b-144">En la carpeta *Schedules*, cree una página denominada *Index.cshtml* para cargar una programación con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="bae3b-144">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="bae3b-145">Cada grupo del formulario incluye una **\<etiqueta>** que muestra el nombre de cada propiedad de clase.</span><span class="sxs-lookup"><span data-stu-id="bae3b-145">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="bae3b-146">Los atributos `Display` del modelo `FileUpload` proporcionan los valores de presentación de las etiquetas.</span><span class="sxs-lookup"><span data-stu-id="bae3b-146">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="bae3b-147">Por ejemplo, el nombre para mostrar de la propiedad `UploadPublicSchedule` se establece con `[Display(Name="Public Schedule")]` y, por tanto, muestra "Programación pública" en la etiqueta cuando se presenta el formulario.</span><span class="sxs-lookup"><span data-stu-id="bae3b-147">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="bae3b-148">Cada grupo del formulario incluye un **\<intervalo>** de validación.</span><span class="sxs-lookup"><span data-stu-id="bae3b-148">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="bae3b-149">Si la entrada del usuario no cumple los atributos de propiedad establecidos en la clase `FileUpload` o si se produce un error en alguna de las comprobaciones de validación del archivo del método `ProcessFormFile`, no se valida el modelo.</span><span class="sxs-lookup"><span data-stu-id="bae3b-149">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="bae3b-150">Cuando se produce un error en la validación del modelo, se presenta un útil mensaje de validación al usuario.</span><span class="sxs-lookup"><span data-stu-id="bae3b-150">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="bae3b-151">Por ejemplo, la propiedad `Title` se anota con `[Required]` y `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="bae3b-151">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="bae3b-152">Si el usuario no proporciona un título, recibe un mensaje que indica que se necesita un valor.</span><span class="sxs-lookup"><span data-stu-id="bae3b-152">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="bae3b-153">Si el usuario especifica un valor de menos de tres caracteres o de más de sesenta caracteres, recibe un mensaje que indica que el valor tiene una longitud incorrecta.</span><span class="sxs-lookup"><span data-stu-id="bae3b-153">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="bae3b-154">Si se proporciona un archivo sin contenido, aparece un mensaje que indica que el archivo está vacío.</span><span class="sxs-lookup"><span data-stu-id="bae3b-154">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-code-behind-file"></a><span data-ttu-id="bae3b-155">Adición del archivo de código subyacente</span><span class="sxs-lookup"><span data-stu-id="bae3b-155">Add the code-behind file</span></span>

<span data-ttu-id="bae3b-156">Agregue el archivo de código subyacente (*Index.cshtml.cs*) a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="bae3b-156">Add the code-behind file (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="bae3b-157">El modelo de página (`IndexModel` en *Index.cshtml.cs*) enlaza la clase `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="bae3b-157">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="bae3b-158">El modelo además usa una lista de las programaciones (`IList<Schedule>`) para mostrar las programaciones almacenadas en la base de datos en la página:</span><span class="sxs-lookup"><span data-stu-id="bae3b-158">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="bae3b-159">Cuando se carga la página con `OnGetAsync`, `Schedules` se rellena a partir de la base de datos y se usa para generar una tabla HTML de programaciones cargadas:</span><span class="sxs-lookup"><span data-stu-id="bae3b-159">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="bae3b-160">Cuando el formulario se publica en el servidor, se activa `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="bae3b-160">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="bae3b-161">Si no es válido, `Schedule` se vuelve a generar y la página se presenta con uno o más mensajes de validación que indican el motivo del error de validación de la página.</span><span class="sxs-lookup"><span data-stu-id="bae3b-161">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="bae3b-162">Si es válido, las propiedades `FileUpload` se usan en *OnPostAsync* para completar la carga de archivos para las dos versiones de la programación y para crear un nuevo objeto `Schedule` para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="bae3b-162">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="bae3b-163">La programación luego se guarda en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="bae3b-163">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="bae3b-164">Vinculación de una página de Razor de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="bae3b-164">Link the file upload Razor Page</span></span>

<span data-ttu-id="bae3b-165">Abra *_Layout.cshtml* y agregue un vínculo a la barra de navegación para llegar a la página de carga de archivos:</span><span class="sxs-lookup"><span data-stu-id="bae3b-165">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="bae3b-166">Adición de una página para confirmar la eliminación de la programación</span><span class="sxs-lookup"><span data-stu-id="bae3b-166">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="bae3b-167">Cuando el usuario hace clic para eliminar una programación, se recomienda ofrecerle una oportunidad de cancelar la operación.</span><span class="sxs-lookup"><span data-stu-id="bae3b-167">When the user clicks to delete a schedule, you want them to have a chance to cancel the operation.</span></span> <span data-ttu-id="bae3b-168">Agregue una página de confirmación de eliminación (*Delete.cshtml*) a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="bae3b-168">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="bae3b-169">El archivo de código subyacente (*Delete.cshtml.cs*) carga una sola programación identificada por `id` en los datos de ruta de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="bae3b-169">The code-behind file (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="bae3b-170">Agregue el archivo *Delete.cshtml.cs* a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="bae3b-170">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="bae3b-171">El método `OnPostAsync` controla la eliminación de la programación mediante su `id`:</span><span class="sxs-lookup"><span data-stu-id="bae3b-171">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="bae3b-172">Después de eliminar correctamente la programación, `RedirectToPage` vuelve a enviar al usuario a la página de programaciones *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bae3b-172">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="bae3b-173">Página de Razor Schedules activa</span><span class="sxs-lookup"><span data-stu-id="bae3b-173">The working Schedules Razor Page</span></span>

<span data-ttu-id="bae3b-174">Cuando se carga la página, las etiquetas y las entradas del título de la programación, la programación pública y la privada se presentan con un botón de envío:</span><span class="sxs-lookup"><span data-stu-id="bae3b-174">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Página de Razor Schedules tal como se muestra en la carga inicial sin errores de validación ni campos vacíos](uploading-files/_static/browser1.png)

<span data-ttu-id="bae3b-176">La selección del botón **Cargar** sin rellenar ninguno de los campos infringe los atributos `[Required]` en el modelo.</span><span class="sxs-lookup"><span data-stu-id="bae3b-176">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="bae3b-177">`ModelState` no es válido.</span><span class="sxs-lookup"><span data-stu-id="bae3b-177">The `ModelState` is invalid.</span></span> <span data-ttu-id="bae3b-178">Los mensajes de error de validación se muestran al usuario:</span><span class="sxs-lookup"><span data-stu-id="bae3b-178">The validation error messages are displayed to the user:</span></span>

![Los mensajes de error de validación aparecen junto a cada control de entrada](uploading-files/_static/browser2.png)

<span data-ttu-id="bae3b-180">Escriba dos letras en el campo **Título**.</span><span class="sxs-lookup"><span data-stu-id="bae3b-180">Type two letters into the **Title** field.</span></span> <span data-ttu-id="bae3b-181">El mensaje de validación cambia para indicar que el título debe tener entre 3 y 60 caracteres:</span><span class="sxs-lookup"><span data-stu-id="bae3b-181">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Mensaje de validación de título modificado](uploading-files/_static/browser3.png)

<span data-ttu-id="bae3b-183">Cuando se cargan una o más programaciones, la sección **Programaciones cargadas** presenta las programaciones cargadas:</span><span class="sxs-lookup"><span data-stu-id="bae3b-183">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabla de programaciones cargadas que muestra el título de cada programación, la fecha de carga en UTC, el tamaño de archivo de versión pública y privada](uploading-files/_static/browser4.png)

<span data-ttu-id="bae3b-185">El usuario puede hacer clic en el vínculo **Eliminar** desde allí para llegar a la vista de confirmación de eliminación, donde tiene una oportunidad de confirmar o cancelar la operación de eliminación.</span><span class="sxs-lookup"><span data-stu-id="bae3b-185">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="bae3b-186">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="bae3b-186">Troubleshooting</span></span>

<span data-ttu-id="bae3b-187">Para más información de solución de problemas de carga de `IFormFile`, vea [Cargas de archivos en ASP.NET Core: Solución de problemas](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="bae3b-187">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="bae3b-188">Gracias por seguir esta introducción a las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="bae3b-188">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="bae3b-189">Le agradeceremos todos los comentarios que quiera hacernos.</span><span class="sxs-lookup"><span data-stu-id="bae3b-189">We appreciate any comments you leave.</span></span> <span data-ttu-id="bae3b-190">[Introducción a MVC y EF Core](xref:data/ef-mvc/intro) es un excelente seguimiento de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="bae3b-190">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bae3b-191">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bae3b-191">Additional resources</span></span>

* [<span data-ttu-id="bae3b-192">Cargas de archivos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bae3b-192">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="bae3b-193">IFormFile</span><span class="sxs-lookup"><span data-stu-id="bae3b-193">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="bae3b-194">Anterior: Validación</span><span class="sxs-lookup"><span data-stu-id="bae3b-194">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)