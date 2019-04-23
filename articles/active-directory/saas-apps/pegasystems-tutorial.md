---
title: 'Tutorial: Integración de Azure Active Directory con Pega Systems | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Pega Systems.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 31acf80f-1f4b-41f1-956f-a9fbae77ee69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 34fe5d85829d909989513214406ba96ea5be0aa8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59271108"
---
# <a name="tutorial-azure-active-directory-integration-with-pega-systems"></a>Tutorial: Integración de Azure Active Directory con Pega Systems

En este tutorial, aprenderá a integrar Pega Systems con Azure Active Directory (Azure AD).
La integración de Pega Systems con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Pega Systems.
* Puede permitir que los usuarios inicien sesión automáticamente en Pega Systems (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Pega Systems, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en Pega Systems

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Pega Systems admite el inicio de sesión único iniciado por **SP** e **IDP**

## <a name="adding-pega-systems-from-the-gallery"></a>Adición de Pega Systems desde la galería

Para configurar la integración de Pega Systems en Azure AD, será preciso que agregue Pega Systems desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Pega Systems desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Pega Systems**, seleccione **Pega Systems** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Pega Systems en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Pega Systems con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Pega Systems.

Para configurar y probar el inicio de sesión único de Azure AD con Pega Systems, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Pega Systems](#configure-pega-systems-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba en Pega Systems](#create-pega-systems-test-user)**: para tener un homólogo de Britta Simon en Pega Systems que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Pega Systems, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Pega Systems**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Pega Systems](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/sp/<INSTANCEID>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/PRRestService/WebSSO/SAML/AssertionConsumerService`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Pega Systems](common/both-advanced-urls.png)

     a. En el cuadro de texto **Dirección URL de inicio de sesión**, escriba el valor de la dirección URL de inicio de sesión.

    b. En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL que siga este patrón: `https://<CUSTOMERNAME>.pegacloud.io/prweb/sso`

    > [!NOTE]
    > Estos valores no son reales. Actualícelos con el identificador real, la dirección URL de respuesta, la dirección URL de inicio de sesión y la dirección URL del estado de la retransmisión. Puede encontrar los valores de Identificador y URL de respuesta de la aplicación Pega que se explica más adelante en este tutorial. Para Estado de la retransmisión, póngase en contacto con el [equipo de soporte técnico del cliente de Pega Systems](https://www.pega.com/contact-us) para obtener el valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

6. La aplicación Pega Systems espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono  **Editar**  para abrir el cuadro de diálogo  **Atributos de usuario** .

    ![imagen](common/edit-attribute.png)

7. Además de lo anterior, la aplicación Pega Systems espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes:

    | NOMBRE | Atributo de origen|
    | ------------------- | -------------------- |
    | uid | *********** |
    | cn  | *********** |
    | mail | *********** |
    | accessgroup | *********** |
    | organization | *********** |
    | orgdivision | *********** |
    | orgunit | *********** |
    | workgroup | *********** |
    | Teléfono | *********** |

    > [!NOTE]
    > Se trata de valores específicos del cliente. Proporcione los valores adecuados.

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

9. En la sección **Set up Pega Systems** (Configurar Pega Systems), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-pega-systems-single-sign-on"></a>Configuración del inicio de sesión único de Pega Systems

1. Para configurar el inicio de sesión único en **Pega Systems**, abra el **portal de Pega** con cuenta de administrador en otra ventana del explorador.

2. Seleccione **Create** -> **SysAdmin** -> **Authentication Service** (Crear > SysAdmin > Servicio de autenticación).

    ![Botón Configurar inicio de sesión único](./media/pegasystems-tutorial/tutorial_pegasystems_admin.png)
    
3. Realice las acciones siguientes en la pantalla **Create Authentication Service** (Crear servicio de autenticación):

    ![Botón Configurar inicio de sesión único](./media/pegasystems-tutorial/tutorial_pegasystems_admin1.png)

     a. Seleccione **SAML 2.0** en Tipo.

    b. En el cuadro de texto **Nombre**, escriba cualquier nombre, por ejemplo, SSO de Azure AD.

    c. En el cuadro de texto **Descripción breve**, escriba cualquier descripción.  

    d. Haga clic en **Create and open** (Crear y abrir). 
    
4. En la sección **Identity Provider (IdP) information** (Información de proveedor de identidades [IdP]), haga clic en **Import IdP metadata** (Importar metadatos de IdP) y busque el archivo de metadatos que ha descargado desde Azure Portal. Haga clic en **Enviar** para cargar los metadatos.

    ![Botón Configurar inicio de sesión único](./media/pegasystems-tutorial/tutorial_pegasystems_admin2.png)
    
5. Esto rellenará los datos del proveedor de identidades tal como se muestra a continuación.

    ![Botón Configurar inicio de sesión único](./media/pegasystems-tutorial/tutorial_pegasystems_admin3.png)
    
6. Realice las siguientes acciones en la sección **Service Provider (SP) settings** (Configuración de proveedor de servicio [SP]):

    ![Botón Configurar inicio de sesión único](./media/pegasystems-tutorial/tutorial_pegasystems_admin4.png)

     a. Copie el valor de **Entity Identification** (Identificación de la entidad) y péguelo en el cuadro de texto **Identificador** de **Configuración básica de SAML** en Azure Portal.

    b. Copie el valor de **Assertion Consumer Service (ACS) location** (Ubicación del servicio de consumidor de aserciones [ACS]) y péguelo en el cuadro de texto **URL de respuesta** de **Configuración básica de SAML** en Azure Portal.

    c. Seleccione **Disable request signing** (Deshabilitar firma de solicitudes).

7. Haga clic en **Guardar**

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba brittasimon@yourcompanydomain.extension. Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Pega Systems.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Pega Systems**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Pega Systems**.

    ![Vínculo a Pega Systems en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-pega-systems-test-user"></a>Creación de un usuario de prueba de Pega Systems

El objetivo de esta sección es crear un usuario llamado Britta Simon en Pega Systems. Trabaje con el [equipo de soporte técnico del cliente de Pega Systems](https://www.pega.com/contact-us) para crear usuarios en Pega Systems.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Pega Systems en el panel de acceso, debería iniciar sesión automáticamente en la versión de Pega Systems para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)