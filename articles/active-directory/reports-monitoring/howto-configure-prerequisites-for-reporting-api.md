---
title: Requisitos previos para acceder a la API de generación de informes de Azure Active Directory | Microsoft Docs
description: Conozca los requisitos previos para acceder a la API de generación de informes de Azure AD.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ab30dfeccc4ae1c7b09a8f48846ffb9e71cfc23
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58436751"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Requisitos previos para acceder a la API de generación de informes de Azure Active Directory

Las [API de generación de informes de Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) proporcionan acceso mediante programación a los datos a través de un conjunto de API de REST. Estas API pueden llamarse desde una variedad de lenguajes de programación y herramientas.

La API de generación de informes utiliza [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) para autorizar el acceso a las API web.

Para preparar el acceso a la API de generación de informes, necesita lo siguiente:

1. [Asignar roles](#assign-roles)
2. [Registro de una aplicación](#register-an-application)
3. [Concesión de permisos](#grant-permissions)
4. [Recopilación de configuraciones](#gather-configuration-settings)

## <a name="assign-roles"></a>Asignación de roles

Para acceder a los datos de informes a través de la API, debe tener asignado uno de los siguientes roles:

- Lector de seguridad

- Administrador de seguridad

- Administrador global


## <a name="register-an-application"></a>Registro de una aplicación

Debe registrar una aplicación, incluso si accede a la API de generación de informes mediante un script. De esta forma se le proporciona un **identificador de la aplicación**, que es necesario para las llamadas de autorización y permite que el código reciba tokens.

Si quiere configurar el directorio para que acceda a la API de generación de informes de Azure AD, debe iniciar sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador de Azure que también sea miembro del rol de directorio **Administrador global** en su inquilino de Azure AD.

> [!IMPORTANT]
> Las aplicaciones que se ejecutan con credenciales con privilegios de administrador pueden ser muy avanzadas, así que asegúrese de mantener seguras las credenciales del secreto o el identificador de la aplicación.
> 

**Registro de una aplicación de Azure AD**

1. En [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory** en el panel de navegación izquierdo.
   
    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. En la página **Azure Active Directory**, haga clic en **Registros de aplicaciones**.

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. En el panel **Registros de aplicaciones**, haga clic en **Nuevo registro de aplicaciones**.

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. En la página **Crear**, realice los pasos siguientes:

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/04.png)

     a. En el cuadro de texto **Nombre**, escriba `Reporting API application`.

    b. Seleccione **Aplicación web o API** como **Tipo de aplicación**.

    c. En el cuadro de texto **URL de inicio de sesión**, escriba `https://localhost`.

    d. Seleccione **Crear**. 


## <a name="grant-permissions"></a>Concesión de permisos 

En función de la API a la que desea acceder, debe conceder los siguientes permisos a la aplicación:  

| API | Permiso |
| --- | --- |
| Microsoft Azure Active Directory | Leer datos de directorio |
| Microsoft Graph | Leer todos los datos del registro de auditoría |


![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/36.png)

En la siguiente sección se enumeran los pasos para ambas API. Si no desea acceder a una de las API, puede omitir los pasos relacionados.

**Para conceder a la aplicación permisos para usar las API:**

1. Seleccione la aplicación en la página **Registros de aplicaciones** y seleccione **Configuración**. 

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. En la hoja **Configuración**, seleccione **Permisos necesarios**. 

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. En la página **Permisos necesarios**, en la lista **API**, haga clic en **Microsoft Azure Active Directory**. 

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. En la página **Habilitar acceso**, seleccione **Leer datos de directorio** y anule la selección de **Iniciar sesión y leer el perfil del usuario**. 

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. En la barra de herramientas de la parte superior, haga clic en **Guardar**.

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/15.png)

6. En la página **Permisos necesarios**, en la barra de herramientas de la parte superior, haga clic en **Agregar**.

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/32.png)

7. En la página **Agregar acceso de API**, haga clic en **Seleccionar una API**.

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/31.png)

8. En la página **Seleccionar una API**, haga clic en **Microsoft Graph** y, a continuación, haga clic en **Seleccionar**.

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/33.png)

9. En la página **Habilitar acceso**, seleccione **Read all audit log data** (Leer todos los datos del registro de auditoría) y, a continuación, haga clic en **Seleccionar**.  

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/34.png)

10. En la página **Agregar acceso de API**, haga clic en **Listo**.  

11. En la página **Permisos necesarios**, en la barra de herramientas de la parte superior, haga clic en **Conceder permisos** y, a continuación, haga clic en **Sí**.

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/17.png)


## <a name="gather-configuration-settings"></a>Recopilación de configuraciones 

En esta sección se muestra cómo obtener la siguiente configuración del directorio:

- Nombre de dominio
- id. de cliente
- Secreto del cliente

Necesitará estos valores al configurar llamadas a la API de generación de informes. 

### <a name="get-your-domain-name"></a>Obtención del nombre de dominio

**Para obtener el nombre del dominio:**

1. En el panel de navegación izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory**.
   
    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. En la página **Azure Active Directory**, seleccione **Nombres de dominio personalizados**.

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Copie el nombre del dominio en la lista de dominios.


### <a name="get-your-applications-client-id"></a>Obtención del id. de cliente de la aplicación

**Para obtener el id. de cliente de la aplicación:**

1. En el panel de navegación izquierdo de [Azure Portal](https://portal.azure.com), haga clic en **Azure Active Directory**.
   
    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Seleccione la aplicación en la página **Registros de aplicaciones**.

3. En la página de la aplicación, vaya a **Id. de aplicación** y seleccione **Hacer clic para copiar**.

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>Obtención del secreto de cliente de la aplicación
Para obtener el secreto de cliente de la aplicación, debe crear una nueva clave y guardar su valor después, ya que no es posible recuperarla más tarde.

**Para obtener el secreto de cliente de la aplicación:**

1. En el panel de navegación izquierdo de [Azure Portal](https://portal.azure.com), haga clic en **Azure Active Directory**.
   
    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  Seleccione la aplicación en la página **Registros de aplicaciones**.

3. En la página de la aplicación, en la barra de herramientas de la parte superior, seleccione **Configuración**. 

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/05.png)

4. En la página **Configuración**, en la sección **Acceso de API**, haga clic en **Claves**. 

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. En la página **Claves**, siga estos pasos:

    ![Registre la aplicación](./media/howto-configure-prerequisites-for-reporting-api/14.png)

     a. En el cuadro de texto **Descripción**, escriba `Reporting API`.

    b. En **Expiración**, seleccione **En 2 años**.

    c. Haga clic en **Save**(Guardar).

    d. Copie el valor de la clave.

## <a name="troubleshoot-errors-in-the-reporting-api"></a>Solución de errores de la API de generación de informes

En esta sección se enumeran los mensajes de error más comunes con los que puede encontrarse al acceder a los informes de actividad mediante la MS Graph API y los pasos para su resolución.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>Error de servidor interno HTTP 500 al obtener acceso al punto de conexión de Microsoft Graph V2

Actualmente no se admite el punto de conexión Microsoft Graph v2; asegúrese de acceder a los registros de actividad mediante el punto de conexión Microsoft Graph v1.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Error: No se pudieron obtener los roles de usuario de Graph AD

Es posible que aparezca este mensaje de error al intentar acceder a los inicios de sesión mediante el Probador de Graph. Asegúrese de que ha iniciado sesión en su cuenta con los dos botones de inicio de sesión de la interfaz de usuario del Probador de Graph, tal como se muestra en la imagen siguiente. 

![Explorador de Graph](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Error: No se pudo realizar la comprobación de licencia Premium de AD Graph 

Si se encuentra con este mensaje de error al intentar acceder a los inicios de sesión mediante el Probador de Graph, seleccione **Modificar permisos** debajo de su cuenta en la barra de navegación de la izquierda y seleccione **Tasks.ReadWrite** y **Directory.Read.All**. 

![Modificación de la interfaz de usuario de permisos](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Error: El inquilino no es B2C o el inquilino no tiene licencia Premium

El acceso a los informes de inicio de sesión requiere una licencia Premium 1 (P1) de Azure Active Directory. Si ve este mensaje de error al acceder a los inicios de sesión, asegúrese de que su inquilino tiene una licencia P1 de Azure AD.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Error: El usuario no está en los roles permitidos 

Si ve este mensaje de error mientras intenta acceder a los registros de auditoría o a los inicios de sesión mediante la API, asegúrese de que su cuenta forma parte del rol **Lector de seguridad** o **Lector de informes** en su inquilino de Azure Active Directory. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Error: Falta el permiso de AAD de la aplicación "Leer datos de directorio" 

Siga los pasos del artículo [Requisitos previos para acceder a la API de generación de informes de Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md) para asegurarse de que la aplicación se ejecuta con el conjunto correcto de permisos. 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Error: Falta el permiso de MSGraph API "Leer todos los datos de registro de auditoría"

Siga los pasos del artículo [Requisitos previos para acceder a la API de generación de informes de Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md) para asegurarse de que la aplicación se ejecuta con el conjunto correcto de permisos. 

## <a name="next-steps"></a>Pasos siguientes

* [Obtención de datos mediante la API de generación de informes de Azure Active Directory con certificados](tutorial-access-api-with-certificates.md)
* [Referencia de la API de auditoría](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Referencia de la API de generación de informes de actividad de inicio de sesión](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
