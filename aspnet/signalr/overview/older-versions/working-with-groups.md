---
uid: signalr/overview/older-versions/working-with-groups
title: Trabajar con grupos en SignalR 1.x | Documentos de Microsoft
author: pfletcher
description: "Este tema describe cómo conservar la información de pertenencia de grupo con la API de concentrador."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 04da74f23663313e70e54fd4f2f9e5f005791cff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="working-with-groups-in-signalr-1x"></a>Trabajar con grupos en SignalR 1.x
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> En este tema se describe cómo agregar usuarios a grupos y conservar la información de pertenencia a grupo.


## <a name="overview"></a>Información general

Grupos de SignalR proporcionan un método para difundir mensajes a subconjuntos especificados de los clientes conectados. Un grupo puede tener cualquier número de clientes y un cliente puede ser un miembro de cualquier número de grupos. No tienes que crear explícitamente grupos. De hecho, un grupo se crea automáticamente la primera vez que especifique su nombre en una llamada a Groups.Add y se elimina cuando se quita la última conexión de pertenencia en ella. Para obtener una introducción al uso de grupos, consulte [cómo administrar la pertenencia a grupos de la clase de base de datos central](index.md) en la API de concentradores - Guía de servidor.

No hay ninguna API para obtener una lista de pertenencia a grupos o una lista de grupos. SignalR envía mensajes a los clientes y grupos basados en un modelo de pub/sub, y el servidor no mantiene las listas de grupos o las pertenencias a grupos. Esto ayuda a maximizar la escalabilidad, ya que cada vez que agregue un nodo a una granja de servidores web, cualquier estado que mantiene SignalR debe se propaguen en el nuevo nodo.

Al agregar un usuario a un grupo con el `Groups.Add` método, el usuario recibe mensajes dirigidos a ese grupo para la duración de la conexión actual, pero la pertenencia del usuario de ese grupo no se conserva más allá de la conexión actual. Si desea conservar permanentemente la información sobre grupos y pertenencia a grupos, debe almacenar esos datos en un repositorio como una base de datos o almacenamiento de tabla de Azure. A continuación, cada vez que un usuario se conecta a la aplicación, recuperar desde el repositorio de qué grupos pertenece el usuario y agrega manualmente ese usuario a esos grupos.

Cuando vuelve a conectar después de una interrupción temporal, el usuario vuelve a une automáticamente los grupos asignados previamente. Automáticamente nueva unión a un grupo solo se aplica cuando vuelve a conectar, no al establecer una conexión nueva. Se pasa un token firmado digitalmente desde el cliente que contiene la lista de grupos asignados previamente. Si desea comprobar si el usuario pertenece a los grupos solicitados, puede invalidar el comportamiento predeterminado.

En este tema, se incluyen las siguientes secciones:

- [Agregar y quitar usuarios](#add)
- [Al llamar a los miembros de un grupo](#call)
- [Almacenamiento de pertenencia a grupos en una base de datos](#storedatabase)
- [Almacenamiento de pertenencia a grupos de almacenamiento de tabla de Azure](#storeazuretable)
- [Comprobar la pertenencia al grupo cuando vuelve a conectar](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Agregar y quitar usuarios

Para agregar o quitar usuarios de un grupo, se llama a la [agregar](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [quitar](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos y pase el identificador del usuario conexión y nombre del grupo como parámetros. No es necesario quitar manualmente un usuario de un grupo cuando finaliza la conexión.

El siguiente ejemplo se muestra la `Groups.Add` y `Groups.Remove` métodos utilizados en los métodos de concentrador.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

El `Groups.Add` y `Groups.Remove` métodos ejecutan de forma asincrónica.

Si desea agregar a un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, se tiene que asegurarse de que el método Groups.Add finaliza primero. Ejemplos de código siguientes muestran cómo hacerlo, uno con un código que funciona en .NET 4.5 y otro con un código que funciona en .NET 4.

#### <a name="asynchronous-net-45-example"></a>Asincrónica .NET 4.5 ejemplo

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Asincrónica .NET 4 ejemplo

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

En general, no es necesario incluir `await` al llamar a la `Groups.Remove` método porque el identificador de conexión que está intentando quitar ya no estén disponible. En ese caso, `TaskCanceledException` se produce cuando se agota el tiempo de espera de la solicitud. Si la aplicación debe asegurarse de que el usuario se ha quitado del grupo antes de enviar un mensaje al grupo, puede agregar `await` antes de Groups.Remove y, a continuación, detectar el `TaskCanceledException` excepción que se puede iniciar.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Al llamar a los miembros de un grupo

Puede enviar mensajes a todos los miembros de un grupo o solo los miembros especificados del grupo, tal como se muestra en los ejemplos siguientes.

- **Todos los** conectado los clientes en un grupo especificado. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Todos los clientes en un grupo especificado conectados **excepto los clientes especificados**, identificado por el identificador de conexión. 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Todos los clientes en un grupo especificado conectados **excepto el cliente que realiza la llamada**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Almacenamiento de pertenencia a grupos en una base de datos

Los ejemplos siguientes muestran cómo conservar la información de usuario y de grupo en una base de datos. Puede usar cualquier tecnología de acceso a datos; Sin embargo, en el ejemplo siguiente muestra cómo definir los modelos que usan Entity Framework. Estos modelos de entidad corresponden a los campos y tablas de base de datos. La estructura de datos puede variar considerablemente según los requisitos de la aplicación. Este ejemplo incluye una clase denominada `ConversationRoom` que debería ser único para una aplicación que permite a los usuarios para unirse a las conversaciones sobre temas diferentes, como deportes o matriz de procesos. En este ejemplo también incluye una clase para las conexiones. La clase de conexión no es absolutamente necesaria para realizar el seguimiento de pertenencia a grupos, pero suele ser parte de una solución sólida para los usuarios de seguimiento.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

A continuación, en el concentrador, puede recuperar la información de grupo y usuario de la base de datos y agregar manualmente el usuario a los grupos adecuados. El ejemplo no incluye código para realizar el seguimiento de las conexiones de usuario. En este ejemplo, el `await` palabra clave no se aplica antes de `Groups.Add` porque no se envía inmediatamente un mensaje a los miembros del grupo. Si desea enviar un mensaje a todos los miembros del grupo inmediatamente después de agregar el nuevo miembro, sería conveniente aplicar el `await` palabra clave para asegurarse de que se complete la operación asincrónica.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Almacenamiento de pertenencia a grupos de almacenamiento de tabla de Azure

Es similar al uso de una base de datos mediante almacenamiento de tabla de Azure para almacenar información de usuario y de grupo. En el ejemplo siguiente se muestra una entidad de tabla que almacena el nombre de usuario y el nombre del grupo.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

En el concentrador, recuperar los grupos asignados cuando se conecta el usuario.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Comprobar la pertenencia al grupo cuando vuelve a conectar

De forma predeterminada, SignalR volver a asigna automáticamente un usuario a los grupos correspondientes cuando vuelve a conectar desde una interrupción temporal, por ejemplo, cuando una conexión se quita y se restablece antes agote el tiempo de espera de la conexión. Información del grupo del usuario se pasa en un token cuando vuelve a conectar, y ese token se comprueba en el servidor. Para obtener información sobre el proceso de comprobación para volver a unir a los usuarios a grupos, consulte [nueva unión a grupos cuando vuelve a conectar](index.md).

En general, debe usar el comportamiento predeterminado de automáticamente nueva unión a grupos en volver a conectar. Grupos de SignalR no están diseñados como un mecanismo de seguridad para restringir el acceso a información confidencial. Sin embargo, si la aplicación debe comprobar la pertenencia a grupos del usuario cuando vuelve a conectar, puede invalidar el comportamiento predeterminado. Cambiar el comportamiento predeterminado puede agregar una carga a la base de datos porque se debe recuperar la pertenencia a grupos del usuario para cada nueva conexión en lugar de simplemente cuando el usuario se conecta.

Si debe comprobar la pertenencia a grupos de volver a conectarse, cree un nuevo módulo de canalización de concentrador que devuelve una lista de grupos asignados, tal y como se muestra a continuación.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

A continuación, agregue ese módulo a la canalización de concentrador, como se indica a continuación.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
