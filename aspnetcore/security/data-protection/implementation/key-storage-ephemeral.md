---
title: "Proveedores de protección de datos efímeros"
author: rick-anderson
description: "Este documento explica los detalles de implementación de los proveedores de protección de datos efímeros ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9c1d03373c9d7fb6dffb3583c58aa593fd3875f4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="ephemeral-data-protection-providers"></a>Proveedores de protección de datos efímeros

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Existen escenarios donde una aplicación necesita un throwaway `IDataProtectionProvider`. Por ejemplo, solo se puede experimentar el desarrollador en una aplicación de consola de uso único o la propia aplicación es transitoria (se incluye en el script o una prueba unitaria de proyecto). Para admitir estos escenarios el [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) paquete incluye un tipo `EphemeralDataProtectionProvider`. Este tipo proporciona una implementación básica de `IDataProtectionProvider` cuya clave repositorio se mantiene solamente en memoria y no escribe en ningún almacén de respaldo.

Cada instancia de `EphemeralDataProtectionProvider` usa su propia clave principal único. Por lo tanto, si un `IDataProtector` con raíz en un `EphemeralDataProtectionProvider` genera una carga protegida, ese carga solo puede desproteger un equivalente `IDataProtector` (les proporciona el mismo [propósito](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) cadena) con raíz en el mismo `EphemeralDataProtectionProvider` instancia.

El siguiente ejemplo muestra cómo crear instancias de un `EphemeralDataProtectionProvider` y usarla para proteger y desproteger los datos.

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
