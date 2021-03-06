---
title: Configurar el tipo de datos de clave principal de identidad
author: AdrienTorris
description: "En este artículo se describe los pasos para configurar el tipo de datos que desee utilizado para la clave principal de ASP.NET Core Identity."
ms.author: scaddie
manager: wpickett
ms.date: 09/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: da46aa90a434a978a55467da982d746eb2d959ee
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a>Configurar el tipo de datos de clave principal de ASP.NET Core Identity

Identidad de ASP.NET Core le permite configurar el tipo de datos que se utiliza para representar una clave principal. Identidad utiliza el `string` tipo de datos de forma predeterminada. Puede invalidar este comportamiento.

## <a name="customize-the-primary-key-data-type"></a>Personalizar el tipo de datos de clave principal

1. Crear una implementación personalizada de la [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) clase. Representa el tipo que se usará para crear objetos de usuario. En el ejemplo siguiente, el valor predeterminado `string` tipo se reemplaza con `Guid`.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. Crear una implementación personalizada de la [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) clase. Representa el tipo que se usará para crear objetos de rol. En el ejemplo siguiente, el valor predeterminado `string` tipo se reemplaza con `Guid`.
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. Cree una clase de contexto de base de datos personalizada. Hereda de la clase de contexto de base de datos de Entity Framework usada para la identidad. El `TUser` y `TRole` argumentos hacen referencia a las clases de usuario y el rol personalizadas creadas en el paso anterior, respectivamente. El `Guid` se define el tipo de datos para la clave principal.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. Registre la clase de contexto de base de datos personalizada al agregar el servicio de identidad en la clase de inicio de la aplicación.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    El `AddEntityFrameworkStores` método no aceptar un `TKey` argumento tal como se hacía en ASP.NET Core 1.x. Tipo de datos de la clave principal se deduce mediante el análisis de la `DbContext` objeto.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    El `AddEntityFrameworkStores` método acepta un `TKey` argumento que indica el tipo de datos de la clave principal.
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a>Probar los cambios

Tras la finalización de los cambios de configuración, la propiedad que representa la clave principal refleja el nuevo tipo de datos. En el ejemplo siguiente se muestra cómo obtener acceso a la propiedad en un controlador MVC.

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
