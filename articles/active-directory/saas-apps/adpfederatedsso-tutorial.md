---
title: 'Tutorial: Integración de Azure Active Directory con ADP | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y ADP.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/04/2019
ms.author: jeedes
ms.openlocfilehash: 981f4d78a351b6d2b61253407e5021dbeb285204
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819550"
---
# <a name="tutorial-azure-active-directory-integration-with-adp"></a>Tutorial: Integración de Azure Active Directory con ADP

En este tutorial, aprenderá a integrar ADP con Azure Active Directory (Azure AD).
La integración de ADP con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a ADP.
* Puede permitir que los usuarios inicien sesión automáticamente en ADP (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ADP, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en ADP

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* ADP admite inicio de sesión único iniciado por **IDP**.

## <a name="adding-adp-from-the-gallery"></a>Adición de ADP desde la galería

Para configurar la integración de ADP en Azure AD, será preciso agregar ADP desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ADP desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **ADP**, seleccione **ADP** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![ADP en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con ADP con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ADP.

Para configurar y probar el inicio de sesión único de Azure AD con ADP, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de ADP](#configure-adp-single-sign-on)**: para configurar el inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de ADP](#create-adp-test-user)**: para tener un homólogo de Britta Simon en ADP que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con ADP, siga estos pasos:

1. En Azure Portal, vaya a la página de integración de la aplicación **ADP**, haga clic en la **pestaña Propiedades** y realice los pasos siguientes: 

    ![Propiedades del inicio de sesión único](./media/adpfederatedsso-tutorial/tutorial_adp_prop.png)

     a. Establezca el valor del campo **Habilitado para que los usuarios inicien sesión** en **Sí**.

    b. Copie la **dirección URL de acceso de usuario** y péguela en la **sección Configurar dirección URL de inicio de sesión** (este paso se explica más adelante en el tutorial).

    c. Establezca el valor del campo **Asignación de usuarios necesaria** en **Sí**.

    d. Establezca el valor del campo **Visible para los usuarios** en **No**.

2. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **ADP**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

3. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

5. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de ADP](common/idp-identifier.png)

    En el cuadro de texto **Identificador (Id. de entidad)**, escriba una dirección URL: `https://fed.adp.com`

6. La aplicación ADP espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**. El nombre de la notificación siempre será **"PersonImmutableID"** cuyo valor se ha asignado a **employeeid**. 

    Se realizará la asignación de usuario de Azure AD a ADP en el valor **employeeid**, pero puede asignarle un valor diferente según la configuración de la aplicación. Por tanto, colabore primero con el **equipo de soporte técnico de ADP** para usar el identificador correcto de un usuario y asignar ese valor a la notificación ["PersonImmutableID"](https://www.adp.com/contact-us/overview.aspx).

    ![imagen](common/edit-attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    | NOMBRE | Atributo de origen | 
    | ---------------| --------------- |
    | PersonImmutableID  | user.employeeid |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

    > [!NOTE] 
    > Antes de configurar la aserción SAML, debe ponerse en contacto con el [equipo de soporte técnico de ADP](https://www.adp.com/contact-us/overview.aspx) y solicitar el valor del atributo de identificador de usuario único del inquilino. Necesitará este valor para configurar la notificación personalizada para su aplicación. 

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

### <a name="configure-adp-single-sign-on"></a>Configuración del inicio de sesión único de ADP

Para configurar el inicio de sesión único en **ADP**, debe cargar el archivo **XML de metadatos** que descargó en el [sitio web de ADP](https://adpfedsso.adp.com/public/login/index.fcc).

> [!NOTE]  
> Este proceso puede tardar unos días.

### <a name="configure-your-adp-services-for-federated-access"></a>Configurar los servicios de ADP para obtener acceso federado

>[!Important]
> Los empleados que necesiten obtener acceso federado a los servicios de ADP deben asignarse a la aplicación de servicio de ADP y, posteriormente, deben volver a asignarse al servicio ADP específico.
Una vez recibida la confirmación del representante de ADP, configure los servicios de ADP y asigne o administre los usuarios para controlar su acceso al servicio de ADP especificado.

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **ADP**, seleccione **ADP** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![ADP en la lista de resultados](common/search-new-app.png)

5. En Azure Portal, vaya a la página de integración de la aplicación **ADP**, haga clic en la **pestaña Propiedades** y realice los pasos siguientes:  

    ![Propiedades vinculadas de inicio de sesión único](./media/adpfederatedsso-tutorial/tutorial_adp_linkedproperties.png)

     a.  Establezca el valor del campo **Habilitado para que los usuarios inicien sesión** en **Sí**.

    b.  Establezca el valor del campo **Asignación de usuarios necesaria** en **Sí**.

    c.  Establezca el valor del campo **Visible para los usuarios** en **Sí**.

6. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **ADP**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

7. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, elija como **Modo** la opción **Vinculado**. Para vincular la aplicación a **ADP**.

    ![Inicio de sesión único vinculado](./media/adpfederatedsso-tutorial/tutorial_adp_linked.png)

8. Navegue hasta la sección **Configurar la dirección URL de inicio de sesión** y realice los pasos siguientes:

    ![Propiedades del inicio de sesión único](./media/adpfederatedsso-tutorial/tutorial_adp_linkedsignon.png)

     a. Pegue la **dirección URL de acceso de usuario** que copió de la **pestaña Propiedades** anterior (de la aplicación principal ADP).
                                                             
    b. Estas son las 5 aplicaciones que admiten diferentes **direcciones URL del estado de la retransmisión**. Tiene que anexar de forma manual el valor adecuado de la **dirección URL del estado de la retransmisión** de una aplicación en particular a la **dirección URL de acceso de usuarios**.
    
    * **ADP Workforce Now**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP Workforce Now Enhanced Time**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise HR**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Guarde** los cambios.

10. Tras recibir la confirmación del representante de ADP, comience la prueba con uno o dos usuarios.

     a. Asigne unos cuantos usuarios a la aplicación del servicio de ADP para que prueben el acceso federado.

    b. La prueba es correcta cuando los usuarios acceden a la aplicación del servicio de ADP en la galería y pueden obtener acceso a su servicio ADP.
 
11. Cuando se confirme una prueba correcta, asigne el servicio de ADP federado a usuarios individuales o a grupos de usuarios (esto se explica más adelante en el tutorial) e implántelo entre sus empleados.
 
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

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure, para lo cual le concederá acceso a ADP.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, luego, **ADP**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **ADP**.

    ![El vínculo a ADP en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-adp-test-user"></a>Creación de un usuario de prueba de ADP

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en ADP. Colabore con el [equipo de soporte técnico de ADP](https://www.adp.com/contact-us/overview.aspx) para agregar los usuarios a la cuenta de ADP. 

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ADP en el Panel de acceso, debería iniciar sesión automáticamente en la solución ADP para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

