---
title: 'Tutorial: Concesión de acceso a una API web de ASP.NET mediante Azure Active Directory B2C | Microsoft Docs'
description: Tutorial sobre cómo usar Active Directory B2C para proteger una API web de ASP.NET y llamarla desde una aplicación web para ASP.NET.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 339b118e48a01469312a40e6b0652a4ffb90291a
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68347127"
---
# <a name="tutorial-grant-access-to-an-aspnet-web-api-using-azure-active-directory-b2c"></a>Tutorial: Concesión de acceso a una API web de ASP.NET mediante Azure Active Directory B2C

Este tutorial muestra cómo llamar a un recurso de API web protegido de Azure Active Directory (Azure AD) B2C desde una aplicación web para ASP.NET.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Configurar el ejemplo para que use la aplicación

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Complete los pasos y requisitos previos en [Tutorial: Habilitación de la autenticación en una aplicación web mediante Azure Active Directory B2C](active-directory-b2c-tutorials-web-app.md).

## <a name="add-a-web-api-application"></a>Incorporar una aplicación de API web

Los recursos de API web tienen que registrarse en el inquilino antes de que puedan aceptar y responder a solicitudes de recursos protegidos por aplicaciones cliente que presenten un token de acceso.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Aplicaciones** y **Agregar**.
5. Escriba un nombre para la aplicación. Por ejemplo, *webapi1*.
6. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
7. En **Dirección URL de respuesta**, escriba un punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. En este tutorial, el ejemplo se ejecuta localmente y escucha en el puerto `https://localhost:44332`.
8. En **URI de id. de aplicación**, escriba el identificador usado para la API web. Se genera el identificador URI completo, incluido el dominio. Por ejemplo, `https://contosotenant.onmicrosoft.com/api`.
9. Haga clic en **Create**(Crear).
10. En la página de propiedades, registre el identificador de la aplicación que usará cuando configure la aplicación web.

## <a name="configure-scopes"></a>Configuración de ámbitos

Los ámbitos proporcionan una manera de controlar el acceso a los recursos protegidos. La API web utiliza los ámbitos para implementar el control de acceso basado en el ámbito. Por ejemplo, los usuarios de la API web pueden tener ambos accesos de lectura y de escritura, o pueden tener solo acceso de lectura. En este tutorial, utilizará ámbitos para definir los permisos de lectura y escritura para la API web.

1. Seleccione **Aplicaciones** y, a continuación, *webapi1*.
2. Seleccione **Ámbitos publicados**.
3. Como **ámbito**, escriba `Hello.Read` y para la descripción, escriba `Read access to hello`.
4. Como **ámbito**, escriba `Hello.Write` y para la descripción, escriba `Write access to hello`.
5. Haga clic en **Save**(Guardar).

Los ámbitos publicados se pueden utilizar para conceder a una aplicación cliente permiso para la API web.

## <a name="grant-permissions"></a>Concesión de permisos

Para llamar a una API web protegida desde una aplicación, deberá conceder permisos de aplicación a la API. En el tutorial de requisitos previos, ha creado una aplicación web en Azure AD B2C denominada *webapp1*. Puede usar esta aplicación para llamar a la API web.

1. Seleccione **Aplicaciones** y, a continuación, seleccione *webapp1*.
2. Seleccione **Acceso de API** y, a continuación, seleccione **Agregar**.
3. En el menú desplegable **Seleccionar API**, seleccione *webapi1*.
4. En el menú desplegable **Seleccionar ámbitos**, seleccione los ámbitos **Hello.Read** y **Hello.Write** que definió previamente.
5. Haga clic en **OK**.

La aplicación está registrada para llamar a la API web protegida. Un usuario se autentica con Azure AD B2C para utilizar la aplicación. La aplicación de escritorio obtiene una concesión de autorización de Azure AD B2C para acceder a la API web protegida.

## <a name="configure-the-sample"></a>Configuración del ejemplo

Ahora que se ha registrado la API web y tiene ámbitos definidos, debe configurar la API web para usar el inquilino de Azure AD B2C. En este tutorial, configurará una API web de ejemplo. La API web de ejemplo se incluye en el proyecto que descargó en el tutorial indicado en los requisitos previos.

Hay dos proyectos en la solución de ejemplo:

Los dos proyectos siguientes están en la solución de ejemplo:

- **TaskWebApp**: para crear y editar una lista de tareas. El ejemplo utiliza el flujo de usuario de **registro o de inicio de sesión** para que los usuarios se registren o inicien sesión.
- **TaskService**: admite la funcionalidad para crear, leer, actualizar y eliminar la lista de tareas. Azure AD B2C protege la API y TaskWebApp la llama.

### <a name="configure-the-web-application"></a>Configuración de la aplicación web

1. Abra la solución **B2C-WebAPI-DotNet** en Visual Studio.
2. Abra **Web.config** en el proyecto **TaskWebApp**.
3. Para ejecutar la API localmente, utilice la configuración de localhost para **api:TaskServiceUrl**. Cambie el archivo Web.config de la manera siguiente: 

    ```csharp
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

3. Configure el identificador URI de la API. Es el URI que utiliza la aplicación web para realizar la solicitud de API. Configure además los permisos solicitados.

    ```csharp
    <add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/api/" />
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

### <a name="configure-the-web-api"></a>Configuración de la API web

1. Abra **Web.config** en el proyecto **TaskService**.
2. Configure la API para usar el inquilino.

    ```csharp
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

3. Establezca el identificador de cliente en el identificador de aplicación registrado para la API.

    ```csharp
    <add key="ida:ClientId" value="<application-ID>"/>
    ```

4. Actualice la configuración del flujo de usuario con el nombre del flujo de usuario de registro y de inicio de sesión.

    ```csharp
    <add key="ida:SignUpSignInUserFlowId" value="B2C_1_signupsignin1" />
    ```

5. Defina la configuración de los ámbitos para que coincida con lo que ha creado en el portal.

    ```csharp
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

## <a name="run-the-sample"></a>Ejecución del ejemplo

Debe ejecutar tanto el proyecto **TaskWebApp** como el de **TaskService**. 

1. En el Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Establecer proyectos de inicio**. 
2. Seleccione **Proyectos de inicio múltiples**.
3. Cambie la **Acción** de ambos proyectos a **Iniciar**.
4. Haga clic en **Aceptar** para guardar la configuración.
5. Presione **F5** para ejecutar las dos aplicaciones. Cada aplicación se abre en su propia pestaña del explorador. `https://localhost:44316/` es la aplicación web.
    `https://localhost:44332/` es la API web.

6. En la aplicación web, haga clic en **Registrar o en Iniciar sesión** para iniciar sesión en la aplicación web. Use la cuenta que creó anteriormente. 
7. Después de iniciar sesión, haga clic en **Lista de tareas pendientes** y cree el elemento de lista correspondiente.

Al crear un elemento de lista de tareas pendientes, la aplicación web realiza una solicitud a la API web para generar este elemento. Su aplicación web protegida está llamando a la API web protegida en el inquilino de Azure AD B2C.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Configurar el ejemplo para que use la aplicación

> [!div class="nextstepaction"]
> [Tutorial: Adición de proveedores de identidades a las aplicaciones en Azure Active Directory B2C](tutorial-add-identity-providers.md)
