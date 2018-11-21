---
title: Requisitos previos para acceder a la API de generación de informes de Azure Active Directory | Microsoft Docs
description: Conozca los requisitos previos para acceder a la API de generación de informes de Azure AD.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: f72d15707d9f56b9e9b5a5d527d1204007c40afa
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621979"
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


## <a name="next-steps"></a>Pasos siguientes

* [Obtención de datos mediante la API de generación de informes de Azure Active Directory con certificados](tutorial-access-api-with-certificates.md)
* [Referencia de la API de auditoría](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Referencia de la API de generación de informes de actividad de inicio de sesión](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
