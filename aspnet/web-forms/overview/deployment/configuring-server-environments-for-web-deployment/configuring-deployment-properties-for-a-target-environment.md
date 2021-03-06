---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: "Configuración de propiedades de implementación de un entorno de destino | Documentos de Microsoft"
author: jrjlee
description: "En este tema se describe cómo configurar propiedades específicas del entorno con el fin de implementar la solución de póngase en contacto con el Administrador de ejemplo en un entorno de destino específico..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: f27b8376b332ff21185be0fd5c00ced7d40a20bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>Configuración de propiedades de implementación de un entorno de destino
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo configurar las propiedades específicas de entorno con el fin de implementar la solución de póngase en contacto con el Administrador de ejemplo en un entorno de destino específico.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales que utiliza una solución de ejemplo & #x 2014; la [póngase en contacto con el Administrador de](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución & #x 2014; para representar una aplicación web con un nivel de complejidad, incluso una aplicación de ASP.NET MVC 3, Windows realista Servicio de Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), en que el proceso de compilación se controla mediante dos proyectos, archivos de & #x 2014; uno que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="process-overview"></a>Información general del proceso

El archivo de proyecto que va a utilizar para compilar e implementar la solución póngase en contacto con el administrador se divide en dos archivos físicos:

- Uno que contenga universal compilar configuración e instrucciones (la *Publish.proj* archivo).
- Configuración de compilación que contiene específicas del entorno (*Env Dev.proj*, *Stage.proj Env*, y así sucesivamente).

En tiempo de compilación, se combina el archivo de proyecto específicas del entorno adecuada en el universal *Publish.proj* archivo para formar un conjunto completo de las instrucciones de compilación. Puede configurar la implementación a los entornos de destino específico al crear o personalizar los archivos de proyecto específicas del entorno con la configuración que describe el escenario de su propia implementación.

Muchos de estos valores se determinan cómo se configurara el entorno de destino & #x 2014; en concreto, si el servidor web de destino está configurado para usar el servicio Web Deployment Agent (agente remoto) o el controlador de implementación Web. Para obtener más información sobre estos enfoques y para obtener instrucciones sobre cómo elegir el enfoque correcto para su propio entorno, consulte [la elección del enfoque de derecha a la implementación de Web](choosing-the-right-approach-to-web-deployment.md).

El [póngase en contacto con el administrador escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) requiere dos archivos de proyecto específicas del entorno:

- Implementación en un entorno de prueba para desarrolladores (*Dev.proj Env*). El entorno de prueba de desarrollador está configurado para aceptar implementaciones remotas con el agente remoto, como se describe en [escenario: configuración de un entorno de prueba para la implementación Web](scenario-configuring-a-test-environment-for-web-deployment.md). Este archivo debe proporcionar al agente remoto dirección del extremo así como los valores específicos de la ubicación como cadenas de conexión y los extremos de servicio.
- Implementación en un entorno de ensayo (*Stage.proj Env*). El entorno de ensayo está configurado para aceptar las implementaciones remotas con el controlador de implementación Web, como se describe en [escenario: configuración de un entorno de ensayo para la implementación Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Este archivo debe proporcionar la dirección del extremo de controlador de Web implementar, así como los valores de específicos de la ubicación como cadenas de conexión y los extremos del servicio.

Es importante tener en cuenta que no afectan la configuración especificada en el archivo de proyecto específicos del entorno, el contenido de la web paquete propio & #x 2014; en su lugar, controlan cómo se implementa el paquete y qué valores de parámetro se proporcionan cuando el se extrae el paquete. Se va a importar el paquete web en el entorno de producción manualmente, tal y como se describe en [escenario: configurar un entorno de producción para la implementación Web](scenario-configuring-a-production-environment-for-web-deployment.md) y [instalación manual de paquetes de Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), por lo que no importa qué configuración que usó en el archivo de proyecto específicas del entorno al que generó el paquete. El Administrador de Internet Information Services (IIS) le pedirá los valores con parámetros, como las cadenas de conexión y los puntos de conexión de servicio, cuando se importa el paquete.

Para implementar la solución póngase en contacto con el administrador para su propio entorno de destino, puede personalizar este archivo o utilizarlo como plantilla y crear su propio archivo.

**Para configurar opciones de implementación específicas del entorno para la solución póngase en contacto con el administrador**

1. Abra la solución ContactManager WCF en Visual Studio 2010.
2. En el **el Explorador de soluciones** ventana, expanda la **publicar** carpeta, expanda la **EnvConfig** carpeta y, a continuación, haga doble clic en **Env-Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. Reemplazar los valores de propiedad de la *Dev.proj Env* archivo con los valores correctos para su propio entorno de prueba.

    > [!NOTE]
    > La tabla que sigue a este procedimiento, proporciona más información sobre cada una de estas propiedades.
4. Guarde el trabajo y, a continuación, cierre el *Dev.proj Env* archivo.

## <a name="choosing-the-right-deployment-properties"></a>Elegir las propiedades de implementación correcta

Esta tabla describen el propósito de cada propiedad en el archivo de proyecto específicos del entorno de ejemplo, *Dev.proj Env*y proporciona alguna orientación sobre los valores que se debe proporcionar.

| Nombre de la propiedad | Detalles |
| --- | --- |
| **MSDeployComputerName** el nombre del extremo de servicio o servidor del web de destino. | Si va a implementar con el servicio del agente remoto en el servidor web de destino, puede especificar el nombre de equipo de destino (por ejemplo, **TESTWEB1** o **TESTWEB1.fabrikam.net**), o puede especificar el servidor remoto punto de conexión de agente (por ejemplo, `http://TESTWEB1/MSDEPLOYAGENTSERVICE`). La implementación funciona del mismo modo en cada caso. Si va a implementar en el controlador de implementación Web en el servidor web de destino, debe especificar el extremo de servicio e incluir el nombre del sitio Web IIS como un parámetro de cadena de consulta (por ejemplo, `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`). |
| **MSDeployAuth** el método que Web Deploy debe usar para autenticarse en el equipo remoto. | Esto debe establecerse en **NTLM** o **básica**. Normalmente, usará **NTLM** si va a implementar con el servicio del agente remoto y **básica** si va a implementar en el controlador de implementación Web. Si utiliza la autenticación básica, debe especificar el nombre de usuario y la contraseña que debe suplantar a la herramienta de implementación Web de IIS (Web Deploy) con el fin de realizar la implementación. En este ejemplo, estos valores se proporcionan a través de la **MSDeployUsername** y **MSDeployPassword** propiedades. Si utiliza la autenticación NTLM, puede omitir estas propiedades o dejarlo en blanco. |
| **MSDeployUsername** si utiliza la autenticación básica, Web Deploy usará esta cuenta en el equipo remoto. | Esto debería tener la forma *dominio*\*nombre de usuario * (por ejemplo, **FABRIKAM\matt**). Este valor solo se utiliza si se especifica la autenticación básica. Si utiliza la autenticación de NTLM, se puede omitir la propiedad. Si se proporciona un valor, se omitirán. |
| **MSDeployPassword** si utiliza la autenticación básica, Web Deploy utilizará esta contraseña en el equipo remoto. | Esta es la contraseña para la cuenta de usuario que especificó en el **MSDeployUsername** propiedad. Este valor solo se utiliza si se especifica la autenticación básica. Si utiliza la autenticación de NTLM, se puede omitir la propiedad. Si se proporciona un valor, se omitirán. |
| **ContactManagerIisPath** ruta de acceso de los IIS en el que desea implementar la aplicación MVC póngase en contacto con el administrador. | Debe ser la ruta de acceso que aparece en el Administrador de IIS, en el formulario [*nombre del sitio Web IIS*] / [*web**nombre de la aplicación*]. Recuerde que el sitio Web IIS debe existir antes de implementar la aplicación. Por ejemplo, si ha creado un sitio Web IIS denominado DemoSite, puede especificar la ruta de acceso IIS para la aplicación MVC como DemoSite/ContactManager. |
| **ContactManagerServiceIisPath** ruta de acceso de los IIS en el que desea implementar el servicio de WCF póngase en contacto con el administrador. | Por ejemplo, si ha creado un sitio Web IIS denominado DemoSite, puede especificar la ruta de acceso IIS para el servicio WCF como **DemoSite/ContactManagerService**. |
| **ContactManagerTargetUrl** la dirección URL a la que puede tener acceso al servicio WCF. | Esto tendrá el formato [*dirección URL raíz del sitio Web IIS*] / [*nombre de la aplicación de servicio*] / [*punto de conexión de servicio*]. Por ejemplo, si ha creado un sitio Web de IIS en el puerto 85, la dirección URL debería tener el formato `http://localhost:85/ContactManagerService/ContactService.svc`. Recuerde que la aplicación MVC y el servicio WCF se implementan en el mismo servidor. Como resultado, esta dirección URL siempre se obtiene acceso desde el equipo en el que está instalado. Por este motivo, es mejor usar localhost o la dirección IP, en lugar del nombre del equipo o un encabezado de host, en la dirección URL. Si usa el nombre del equipo o un encabezado de host, el [comprobación de bucle invertido](https://go.microsoft.com/?linkid=9805131) característica de seguridad de IIS puede bloquear la dirección URL y devolver un **HTTP 401.1 - no autorizado** error. |
| **CmDatabaseConnectionString** la cadena de conexión para el servidor de base de datos. | La cadena de conexión determina tanto las credenciales que utilizará VSDBCMD para ponerse en contacto con el servidor de base de datos y crear la base de datos y las credenciales que utilizará el grupo de aplicaciones de servidor web para ponerse en contacto con el servidor de base de datos e interactuar con la base de datos. Básicamente tiene dos opciones aquí. Puede especificar **Integrated Security = true**, en cuyo caso se utiliza la autenticación integrada de Windows: **origen de datos = TESTDB1; Integrated Security = true** en este caso, se creará la base de datos mediante las credenciales del usuario que ejecuta la herramienta VSDBCMD ejecutable y la aplicación tendrá acceso a la base de datos utilizando la identidad de la cuenta del equipo servidor web. Como alternativa, puede especificar el nombre de usuario y la contraseña de una cuenta de SQL Server. En este caso, se utilizan las credenciales de SQL Server mediante VSDBCMD para crear la base de datos y el grupo de aplicaciones para interactuar con la base de datos: **origen de datos = TESTDB1; Id. de usuario = ASqlUser; Contraseña = Pa$ $w0rd** los tutoriales en este tema se suponen que podrá usar la autenticación integrada de Windows. |
| **CmTargetDatabase** el nombre que desea conceder a la base de datos creará en el servidor de base de datos. | El valor que proporcione aquí se agrega al comando VSDBCMD como un parámetro. También sirve para generar una cadena de conexión completa que puede usar el grupo de aplicaciones en el servidor web para interactuar con la base de datos. |
  

Estos ejemplos muestran cómo puede configurar estas propiedades para escenarios de implementación específicos.

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>Ejemplo 1 & #x 2014; de implementación para el servicio del agente remoto

En este ejemplo:

- Va a implementar con el servicio del agente remoto en TESTWEB1.
- Se está dando a Web Deploy para usar la autenticación NTLM. Web Deploy ejecuta con las credenciales que usó para invocar Microsoft Build Engine (MSBuild).
- Usa la autenticación integrada para implementar la **ContactManager** base de datos para TESTDB1. La base de datos se implementan mediante las credenciales que usó para invocar MSBuild.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>Ejemplo 2 & #x 2014; implementación en la Web implementar punto de conexión de controlador

En este ejemplo:

- Va a implementar en el punto de conexión de servicio de controlador de implementación Web en STAGEWEB1.
- Se está dando a Web Deploy para usar la autenticación básica.
- Se está especificando que Web Deploy debe suplantar la cuenta de FABRIKAM\stagingdeployer en el equipo remoto.
- Utiliza la autenticación de SQL Server para implementar el **ContactManager** STAGEDB1 base de datos.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>Conclusión

En este punto, los archivos de proyecto se configuren totalmente para compilar e implementar la solución póngase en contacto con el administrador para uno o más entornos de destino.

Para utilizar estos archivos de proyecto como parte de un proceso de implementación paso a paso, repetibles, tiene que ejecutar el *Publish.proj* archivo utilizando MSBuild y pase la ubicación del archivo del proyecto específicas del entorno como un parámetro. Puede hacerlo de varias maneras:

- Para obtener información general de MSBuild y una introducción a los archivos de proyecto personalizadas, consulte [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Para obtener información acerca de cómo formular un comando de MSBuild que ejecuta los archivos de proyecto personalizado, consulte [implementar paquetes de Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).
- Para obtener información sobre cómo incorporar los comandos de MSBuild en un archivo de comandos para las implementaciones de paso a paso, repetibles, vea [crear y ejecutar un archivo de comandos de implementación](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).
- Para obtener información sobre cómo ejecutar los archivos de proyecto personalizados de Team Build, consulte [crear una definición de compilación que admite la implementación](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

>[!div class="step-by-step"]
[Anterior](creating-a-server-farm-with-the-web-farm-framework.md)
