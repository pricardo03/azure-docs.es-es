---
title: 'Tutorial: Integración de Azure Active Directory con SAP Cloud for Customer | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SAP Cloud for Customer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.openlocfilehash: 6f2fb29e705edde24d6f54faafd17b580ae131b5
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2018
ms.locfileid: "53809869"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Tutorial: Integración de Azure Active Directory con SAP Cloud for Customer

En este tutorial, aprenderá a integrar SAP Cloud for Customer con Azure Active Directory (Azure AD).
La integración de SAP Cloud for Customer con Azure AD proporciona las siguientes ventajas:

* En Azure AD se puede controlar quién tiene acceso a SAP Cloud for Customer.
* Puede habilitar que los usuarios inicien sesión automáticamente en SAP Cloud for Customer (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SAP Cloud for Customer, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción a SAP Cloud for Customer con inicio de sesión único habilitado

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SAP Cloud for Customer el inicio de sesión único iniciado por **PS**

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>Adición de SAP Cloud for Customer desde la galería

Para configurar la integración de SAP Cloud for Customer en Azure AD, es preciso agregar SAP Cloud for Customer desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SAP Cloud for Customer desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **SAP Cloud for Customer**, seleccione **SAP Cloud for Customer** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![SAP Cloud for Customer en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, se configura y se prueba el inicio de sesión único de Azure AD con SAP Cloud for Customer con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SAP Cloud.

Para configurar y probar el inicio de sesión único de Azure AD con SAP Cloud for Customer, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de SAP Cloud for Customer](#configure-sap-cloud-for-customer-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de SAP Cloud for Customer](#create-sap-cloud-for-customer-test-user)**: para tener un homólogo de Britta Simon en SAP Cloud for Customer que esté vinculado a su representación en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con SAP Cloud for Customer, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **SAP Cloud for Customer**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de SAP Cloud for Customer](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<server name>.crm.ondemand.com`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<server name>.crm.ondemand.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de SAP Cloud for Customer](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. La aplicación SAP Cloud for Customer espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

6. En la sección **Atributos del usuario** del cuadro de diálogo **Atributos y notificaciones de usuario**, siga estos pasos:

     a. Haga clic en el **icono Editar** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/sap-customer-cloud-tutorial/tutorial_usermail.png)

    ![imagen](./media/sap-customer-cloud-tutorial/tutorial_usermailedit.png)

    b. Seleccione **Transformación** como **origen**.

    c. En la lista **Transformación**, seleccione **ExtractMailPrefix()**.

    d. En la lista **Parámetro 1**, seleccione el atributo de usuario que desea usar en la implementación.
    Por ejemplo, si quiere usar EmployeeID como identificador de usuario único y ha almacenado el valor del atributo en ExtensionAttribute2, seleccione user.extensionattribute2.

    e. Haga clic en **Save**(Guardar).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

8. En la sección **Set up SAP Cloud for Customer** (Configurar SAP Cloud for Customer), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-sap-cloud-for-customer-single-sign-on"></a>Configuración del inicio de sesión único de SAP Cloud for Customer
   
1. Inicie sesión en el portal de SAP Cloud for Customer con derechos de administrador.
   
2. Navegue hasta **Application and User Management Common Task** (Tarea común de administración de usuarios y aplicaciones) y haga clic en la pestaña **Proveedor de identidades**.
   
3. Haga clic en **New Identity Provider** (Nuevo proveedor de identidades) y seleccione el archivo XML de metadatos que ha descargado de Azure Portal. Al importar los metadatos, el sistema carga automáticamente el certificado de firma y el certificado de cifrado necesarios.
   
    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
4. Azure Active Directory requiere la URL del Servicio de consumidor de aserciones del elemento en la solicitud SAML, así que active la casilla **Include Assertion Consumer Service URL** (Incluir URL del Servicio de consumidor de aserciones).
   
5. Haga clic en **Activate Single Sign-On**(Activar inicio de sesión único).
   
6. Guarde los cambios.
   
7. Haga clic en la pestaña **My System** (Mi sistema).
   
    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
8. En el cuadro de texto **Azure AD Sign On URL** (Dirección URL de inicio de sesión de Azure), pegue el valor de la **URL de inicio de sesión** que ha copiado de Azure Portal.
   
    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
9. Especifique si el empleado puede elegir manualmente entre iniciar sesión con el Id. de usuario y una contraseña o mediante SSO, para lo que debe seleccionar **Manual Identity Provider Selection** (Selección manual de proveedor de identidades).
   
10. En la sección **SSO URL** (URL de SSO), especifique la dirección URL que deben utilizar los empleados para iniciar sesión en el sistema. 
    En la lista **URL Sent to Employee** (URL que se envía a empleado), puede elegir una de las siguientes opciones:
   
    **Non-SSO URL**
   
    El sistema solo envía la dirección URL del sistema normal al empleado. El empleado no puede iniciar sesión mediante SSO y debe usar una contraseña o un certificado en su lugar.
   
    **SSO URL** 
   
    El sistema solo envía la dirección URL de SSO al empleado. El empleado puede iniciar sesión mediante SSO. La solicitud de autenticación se redirige a través de IdP.
   
    **Automatic Selection**
   
    SI SSO no está activo, el sistema envía la dirección URL del sistema normal al empleado. Si SSO está activo, el sistema comprueba si el empleado tiene contraseña. Si existe una contraseña, se envían al empleado tanto la URL de SSO como la URL no de SSO. Sin embargo, si el empleado no tiene contraseña, solo se envía la primera al empleado.
   
11. Guarde los cambios.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a SAP Cloud for Customer para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **SAP Cloud for Customer**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **SAP Cloud for Customer**.

    ![El vínculo de SAP Cloud for Customer en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-sap-cloud-for-customer-test-user"></a>Creación de un usuario de prueba de SAP Cloud for Customer

En esta sección, creará un usuario llamado Britta Simon en SAP Cloud for Customer. Trabaje con el  [equipo de soporte técnico de SAP Cloud for Customer](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) para agregar usuarios en la plataforma SAP Cloud for Customer. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

> [!NOTE]
> Asegúrese de que el valor de NameID coincide con el campo de nombre de usuario de la plataforma SAP Cloud for Customer.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SAP Cloud for Customer del panel de acceso, debería iniciar sesión automáticamente en la aplicación SAP Cloud for Customer para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

