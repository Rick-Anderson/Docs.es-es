---
title: "Autorización personalizada basada en directivas en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo crear y usar controladores de directiva de autorización personalizada para exigir requisitos de autorización en una aplicación de ASP.NET Core."
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 1067e97dd6e71021929aa3690b0c3f5bfc6c9724
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="5ccbd-103">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="5ccbd-103">Custom policy-based authorization</span></span>

<span data-ttu-id="5ccbd-104">Interiormente, [autorización basada en roles](xref:security/authorization/roles) y [autorización basada en notificaciones](xref:security/authorization/claims) utilizan un requisito, un controlador de requisito y una directiva configurada previamente.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="5ccbd-105">Estos bloques de creación se admite la expresión de evaluaciones de autorización en el código.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="5ccbd-106">El resultado es una estructura de autorización más enriquecida, reutilizables, comprobable.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="5ccbd-107">Una directiva de autorización está formada por uno o más requisitos.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="5ccbd-108">Se registra como parte de la configuración de servicio de autorización, en la `ConfigureServices` método de la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="5ccbd-108">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="5ccbd-109">En el ejemplo anterior, se crea una directiva de "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="5ccbd-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="5ccbd-110">Tiene un requisito único, que de una antigüedad mínima, que se proporciona como un parámetro al requisito.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-110">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="5ccbd-111">Las directivas se aplican mediante el `[Authorize]` atributo con el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="5ccbd-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5ccbd-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="5ccbd-113">Requisitos</span><span class="sxs-lookup"><span data-stu-id="5ccbd-113">Requirements</span></span>

<span data-ttu-id="5ccbd-114">Un requisito de autorización es una colección de parámetros de datos que puede usar una directiva para evaluar la entidad de seguridad del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="5ccbd-115">En nuestra directiva de "AtLeast21", el requisito es un parámetro único&mdash;la antigüedad mínima.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="5ccbd-116">Implementa un requisito `IAuthorizationRequirement`, que es una interfaz de marcador vacío.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-116">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="5ccbd-117">Un requisito de antigüedad mínima con parámetros podría implementarse como sigue:</span><span class="sxs-lookup"><span data-stu-id="5ccbd-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="5ccbd-118">No tiene un requisito que tienen datos o propiedades.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="5ccbd-119">Controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="5ccbd-119">Authorization handlers</span></span>

<span data-ttu-id="5ccbd-120">Un controlador de autorización es responsable de la evaluación de propiedades de un requisito.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="5ccbd-121">El controlador de autorización evalúa los requisitos con proporcionado `AuthorizationHandlerContext` para determinar si se permite el acceso.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-121">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="5ccbd-122">Puede tener un requisito [varios controladores](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="5ccbd-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="5ccbd-123">Heredan de controladores `AuthorizationHandler<T>`, donde `T` es el requisito para procesarse.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-123">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="5ccbd-124">El controlador de antigüedad mínima podría ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="5ccbd-124">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="5ccbd-125">El código anterior determina si la entidad de seguridad del usuario actual tiene una fecha de nacimiento de notificación de que se ha emitido por un emisor conocido y de confianza.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-125">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="5ccbd-126">No se puede realizar la autorización cuando falta la notificación, en cuyo caso se devuelve una tarea completa.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-126">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="5ccbd-127">Cuando está presente una notificación, se calcula la edad del usuario.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-127">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="5ccbd-128">Si el usuario cumple con la antigüedad mínima definida por el requisito, la autorización se considere correcta.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-128">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="5ccbd-129">Cuando se realiza correctamente, la autorización `context.Succeed` se invoca con el requisito satisfecho como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-129">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="5ccbd-130">Registro del controlador</span><span class="sxs-lookup"><span data-stu-id="5ccbd-130">Handler registration</span></span>

<span data-ttu-id="5ccbd-131">Los controladores se registran en la colección de servicios durante la configuración.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-131">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="5ccbd-132">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5ccbd-132">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="5ccbd-133">Cada controlador se agrega a la colección de servicios mediante la invocación de `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-133">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="5ccbd-134">¿Qué debe devolver un controlador?</span><span class="sxs-lookup"><span data-stu-id="5ccbd-134">What should a handler return?</span></span>

<span data-ttu-id="5ccbd-135">Tenga en cuenta que la `Handle` método en el [controlador (ejemplo)](#security-authorization-handler-example) no devuelve ningún valor.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-135">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="5ccbd-136">¿Cómo es un estado de correcto o erróneo indicadas?</span><span class="sxs-lookup"><span data-stu-id="5ccbd-136">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="5ccbd-137">Un controlador indica éxito mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito de que se ha validado correctamente.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-137">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="5ccbd-138">Un controlador no necesita controlar los errores por lo general, como otros controladores para el mismo requisito pueden realizarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-138">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="5ccbd-139">Para garantizar el error, incluso si otros controladores de requisito correctamente, llame a `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-139">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="5ccbd-140">Independientemente de lo que se llama a dentro de su controlador, todos los controladores para un requisito se llamará cuando una directiva exige el requisito.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-140">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="5ccbd-141">Esto permite requisitos tienen efectos secundarios, como el registro, que siempre se llevará a cabo aunque `context.Fail()` se ha llamado en otro controlador.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-141">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="5ccbd-142">¿Por qué desearía varios controladores para un requisito?</span><span class="sxs-lookup"><span data-stu-id="5ccbd-142">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="5ccbd-143">En casos donde probablemente prefiera evaluación en un **o** base, implementar varios controladores para un requisito único.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-143">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="5ccbd-144">Por ejemplo, Microsoft tiene puertas que solo se abren con tarjetas de clave.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-144">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="5ccbd-145">Si deja la tarjeta de claves en casa, la recepcionista imprime una etiqueta temporal y abre la puerta para usted.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-145">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="5ccbd-146">En este escenario, tendría un requisito único, *BuildingEntry*, pero varios controladores, cada uno de ellos examinando un requisito único.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-146">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="5ccbd-147">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="5ccbd-147">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="5ccbd-148">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="5ccbd-148">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="5ccbd-149">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="5ccbd-149">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="5ccbd-150">Asegúrese de que ambos controladores estén [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="5ccbd-150">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="5ccbd-151">Si cualquier controlador se ejecuta correctamente cuando una directiva se evalúa como el `BuildingEntryRequirement`, la evaluación de directivas se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-151">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="5ccbd-152">Usar un elemento func para cumplir una directiva</span><span class="sxs-lookup"><span data-stu-id="5ccbd-152">Using a func to fulfill a policy</span></span>

<span data-ttu-id="5ccbd-153">Puede haber situaciones en las que cumplir una directiva es simple expresar en el código.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-153">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="5ccbd-154">Es posible proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la directiva con el `RequireAssertion` el generador de directiva.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-154">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="5ccbd-155">Por ejemplo, el anterior `BadgeEntryHandler` podría volver a escribir como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="5ccbd-155">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="5ccbd-156">Acceso al contexto de solicitud MVC en los controladores</span><span class="sxs-lookup"><span data-stu-id="5ccbd-156">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="5ccbd-157">El `HandleRequirementAsync` método se implementa en un controlador de autorización tiene dos parámetros: una `AuthorizationHandlerContext` y `TRequirement` está controlando.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-157">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="5ccbd-158">Marcos de trabajo como MVC o Jabbr pueden agregar cualquier objeto a la `Resource` propiedad en el `AuthorizationHandlerContext` para pasar información adicional.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-158">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="5ccbd-159">Por ejemplo, MVC pasa una instancia de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) en el `Resource` propiedad.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-159">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="5ccbd-160">Esta propiedad proporciona acceso a `HttpContext`, `RouteData`y todo el contenido más proporcionó MVC y las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-160">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="5ccbd-161">El uso de la `Resource` propiedad es específicos de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-161">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="5ccbd-162">De manera indicada en el `Resource` propiedad limita las directivas de autorización para marcos de trabajo determinados.</span><span class="sxs-lookup"><span data-stu-id="5ccbd-162">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="5ccbd-163">Debe convertir el `Resource` propiedad mediante la `as` (palabra clave) y, a continuación, confirme la conversión se realizará correctamente para asegurarse de que el código de no bloqueo con un `InvalidCastException` cuando se ejecuta en otros marcos de trabajo:</span><span class="sxs-lookup"><span data-stu-id="5ccbd-163">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```