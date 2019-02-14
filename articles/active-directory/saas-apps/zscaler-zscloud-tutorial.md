---
title: 'Tutorial: Integración de Azure Active Directory con Zscaler ZSCloud | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y ZScaler ZSCloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c70d58669fe6351c67a54c282ee1310fb52ed50a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56218064"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Tutorial: Integración de Azure Active Directory con Zscaler ZSCloud

En este tutorial, aprenderá a integrar ZScaler ZSCloud con Azure Active Directory (Azure AD).

La integración de ZScaler ZSCloud con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Zscaler ZSCloud.
- Puede permitir que los usuarios inicien sesión automáticamente en Zscaler ZSCloud (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ZScaler ZSCloud, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en ZScaler ZSCloud

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de ZScaler ZSCloud desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Incorporación de ZScaler ZSCloud desde la galería

Para configurar la integración de ZScaler ZSCloud en Azure AD, será preciso agregar ZScaler ZSCloud desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ZScaler ZSCloud desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Zscaler ZSCloud**, seleccione **Zscaler ZSCloud** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Zscaler ZSCloud en la lista de resultados](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Zscaler ZSCloud con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de ZScaler ZSCloud para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario asociado de ZScaler ZSCloud.

Para configurar y probar el inicio de sesión único de Azure AD con ZScaler ZSCloud, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de ZScaler ZSCloud](#creating-a-zscaler-zscloud-test-user)**: para tener un homólogo de Britta Simon en ZScaler ZSCloud vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación ZScaler ZSCloud.

**Para configurar el inicio de sesión único de Azure AD con ZScaler ZSCloud, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **ZScaler ZSCloud**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Zscaler ZSCloud](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL con la que los usuarios inician sesión en la aplicación ZScaler ZSCloud.

    > [!NOTE] 
    > Tiene que actualizar este valor con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Zscaler Two](https://help.zscaler.com/) para obtener estos valores.

5. La aplicación Zscaler ZSCloud espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **User Attributes & Claims** (Atributos y notificaciones del usuario) de la página de integración de aplicaciones. En la **página Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario).

    ![Vínculo de atributos](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | NOMBRE  | Atributo de origen  |
    | ---------| ------------ |
    | memberOf     | user.assignedroles |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./common/new_save_attribute.png)
    
    ![imagen](./common/new_attribute_details.png)

    b. En la lista **Atributo de origen**, seleccione el valor del atributo.

    c. Haga clic en **Aceptar**.

    d. Haga clic en **Save**(Guardar).

    > [!NOTE]
    > Haga clic [aquí](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) para saber cómo configurar el valor Role en Azure AD.

7. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

8. En la sección **Configurar Zscaler ZSCloud**, copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de Zscaler ZSCloud](common/configuresection.png)

9. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de Zscaler ZSCloud como administrador.

10. Vaya a **Administración > Autenticación > Configuración de autenticación** y realice los siguientes pasos:
   
    ![Administración](./media/zscaler-zscloud-tutorial/ic800206.png "Administración")

     a. En Tipo de autenticación, elija **SAML**.

    b. Haga clic en **Configurar SAML**.

11. En la ventana **Editar SAML**, realice los pasos siguientes y haga clic en Guardar.  
            
    ![Administración de usuarios y autenticación](./media/zscaler-zscloud-tutorial/ic800208.png "Administración de usuarios y autenticación")
    
     a. En el cuadro de texto **Dirección URL del portal de SAML**, pegue la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    b. En el cuadro de texto **Atributo de nombre de inicio de sesión**, escriba **NameID**.

    c. Haga clic en **Cargar** para cargar el certificado de firma de SAML de Azure que ha descargado desde Azure Portal en el **Certificado SSL público**.

    d. Alterne **Habilitar aprovisionamiento automático de SAML**.

    e. En el cuadro de texto **Atributo de nombre para mostrar del usuario**, escriba **displayName** si desea habilitar el aprovisionamiento automático de SAML para atributos displayName.

    f. En el cuadro de texto **Atributo de nombre del grupo**, escriba **memberOf** si desea habilitar el aprovisionamiento automático de SAML para atributos memberOf.

    g. En el cuadro de texto **Atributo de nombre del departamento**, escriba **department** si desea habilitar el aprovisionamiento automático de SAML para atributos department.

    i. Haga clic en **Save**(Guardar).

12. En la página del cuadro de diálogo **Configurar autenticación de usuario** , realice los pasos siguientes:

    ![Administración](./media/zscaler-zscloud-tutorial/ic800207.png)

     a. Mantenga el puntero sobre el menú **Activación** situado cerca de la parte inferior izquierda.

    b. Haga clic en **Activar**.

## <a name="configuring-proxy-settings"></a>Configuración de los valores de proxy
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Para definir la configuración de proxy en Internet Explorer

1. Inicie **Internet Explorer**.

1. Seleccione **Opciones de Internet** en el menú **Herramientas** para abrir el diálogo **Opciones de Internet**.   
    
     ![Opciones de Internet](./media/zscaler-zscloud-tutorial/ic769492.png "Opciones de Internet")

1. Haga clic en la pestaña **Conexiones** .   
  
     ![Conexiones](./media/zscaler-zscloud-tutorial/ic769493.png "Conexiones")

1. Haga clic en **Configuración de LAN** para abrir el diálogo **Configuración de LAN**.

1. En la sección del servidor proxy, lleve a cabo estos pasos:   
   
    ![Servidor proxy](./media/zscaler-zscloud-tutorial/ic769494.png "Servidor proxy")

     a. Seleccione **Usar un servidor proxy para la LAN**.

    b. En el cuadro de texto Dirección, escriba **gateway.Zscaler ZSCloud.net**.

    c. En el cuadro de texto Puerto, escriba **80**.

    d. Seleccione **No usar servidor proxy para direcciones locales**.

    e. Haga clic en **Aceptar** para cerrar el diálogo **Configuración de red de área local (LAN)**.

1. Haga clic en **Aceptar** para cerrar el diálogo **Opciones de Internet**.

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-a-zscaler-zscloud-test-user"></a>Creación de un usuario de prueba de ZScaler ZSCloud

El objetivo de esta sección es crear un usuario llamado Britta Simon en Zscaler ZSCloud. Zscaler ZSCloud admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Durante un intento de obtener acceso a Zscaler ZSCloud se creará un nuevo usuario, en caso de que no exista.
>[!Note]
>Si tiene que crear un usuario manualmente, póngase en contacto con el  [equipo de soporte técnico de Zscaler ZSCloud](https://help.zscaler.com/).

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a ZScaler ZSCloud.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **ZScaler ZSCloud**.

    ![Configurar inicio de sesión único](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione el usuario, como **Britta Simon**, en la lista y luego haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

    ![imagen](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_users.png)

6. En el cuadro de diálogo **Seleccionar rol**, elija el rol de usuario adecuado de la lista y, luego, haga clic en el botón **Seleccionar** situado en la parte inferior de la pantalla.

    ![imagen](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_roles.png)

7. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

    ![imagen](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_assign.png)

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ZScaler ZSCloud del Panel de acceso, iniciará sesión en la aplicación ZScaler ZSCloud automáticamente.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
